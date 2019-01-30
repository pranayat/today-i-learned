# Creating pm2 ecosystem.config

- array of apps
- ```pm2 start ecosystem.config.js --env staging``` , this will inject environment specified in env_```staging```,
   environment keys to be of the form ```env_<environment name>```
- access then env variables in your app as ```process.env.<VARIABLE_NAME>```
- in script specify relative path to file


```module.exports = {
  /**
   * Application configuration section
   * http://pm2.keymetrics.io/docs/usage/application-declaration/
   */
  apps : [

    // First application
    {
      name      : 'BUZZ',
      script    : 'app.js',
      env: {
        COMMON_VARIABLE: 'true'
      },
      env_staging: {
        NODE_ENV: 'staging',
        MONGODB_URI: '',
        JWT_SECRET: ''
      },
      env_production : {
        NODE_ENV: 'production'
      }
    },

    // Second application
    {
      name      : 'PDF_WORKER',
      script    : 'pdf.worker.driver.js',
      env_staging: {
        NODE_ENV: 'staging',
        MONGODB_URI: '',
        JWT_SECRET: ''
      },
    }
  ],

  /**
   * Deployment section
   * http://pm2.keymetrics.io/docs/usage/deployment/
   */
  deploy : {
    production : {
      user : 'node',
      host : '212.83.163.1',
      ref  : 'origin/master',
      repo : 'git@github.com:repo.git',
      path : '/var/www/production',
      'post-deploy' : 'npm install && pm2 reload ecosystem.config.js --env production'
    },
    dev : {
      user : 'node',
      host : '212.83.163.1',
      ref  : 'origin/master',
```
