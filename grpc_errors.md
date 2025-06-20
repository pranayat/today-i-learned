- gRPC calls return errors which are compatible with the Status type, defined in the package https://pkg.go.dev/google.golang.org/grpc/status.
- gRPC serializes these errors to the Status type using proto buffers to send them over the wire.
  ```
  type Status struct {
	s *spb.Status
  }
  ```
  
  ```
  type Status struct {
  	state         protoimpl.MessageState
  	sizeCache     protoimpl.SizeCache
  	unknownFields protoimpl.UnknownFields
  
  	// The status code, which should be an enum value of
  	// [google.rpc.Code][google.rpc.Code].
  	Code int32 `protobuf:"varint,1,opt,name=code,proto3" json:"code,omitempty"`
  	// A developer-facing error message, which should be in English. Any
  	// user-facing error message should be localized and sent in the
  	// [google.rpc.Status.details][google.rpc.Status.details] field, or localized
  	// by the client.
  	Message string `protobuf:"bytes,2,opt,name=message,proto3" json:"message,omitempty"`
  	// A list of messages that carry the error details.  There is a common set of
  	// message types for APIs to use.
  	Details []*anypb.Any `protobuf:"bytes,3,rep,name=details,proto3" json:"details,omitempty"`
  }
  ```
  
  ```
  // Server returns a status compatible error with gRPC code and message
  return status.Error(codes.NotFound, "product not found")
  ```
  ```
  // gRPC serealizes this as Status
  message Status {
  int32 code = 1;
  string message = 2;
  repeated google.protobuf.Any details = 3;
  }

  // if server had returned errors.New("product not found"), code would be set to Unknown
  ```
  
  ```
  // Client deserializes this to get Status type
  st, ok: status.FromError(err)
  ```
  
- You can convert a gRPC error to a Status `st, ok := status.FromError(err)` and vice versa `err := st.Err()`.
- Although the `Details` field is of type `[]*anypb.Any`, gRPC defines a set of standard detail payloads [here](https://github.com/googleapis/googleapis/blob/master/google/rpc/error_details.proto).
  - ErrorInfo: spec definition https://google.aip.dev/193#errorinfo
  - RetryInfo
  - DebugInfo
  - QuotaFailure
  - PreconditionFailure
  - BadRequest
  - RequestInfo
  - Help
  - LocalizedMessage
 - Eg. Creating a Status with `ErrorInfo` details
   ```
    // create details using type ErrorInfo defined in the errordetails package
    ei := &errdetails.ErrorInfo{
		  Metadata: metadata,
    }
    st := status.New(codes.PermissionDenied, err.Error()) // create a new Status with a gRPC code and error message
    st, err := st.WithDetails(ei) // populate the details field
    if err != nil {
   	...
     }
    return st.Err() // server returns a Status compatible error
   ```
  
