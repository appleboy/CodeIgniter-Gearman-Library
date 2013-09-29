#Codeigniter Gearman Library

This is a library to use gearman within codeigniter. The original source was pulled from Sunil here https://github.com/sunils34/codeigniter-gearman-library. I add some feature and remove deprecated function.

Please make sure you have setuped gearman daemon environment and gearman pecl drivers. (http://pecl.php.net/package/gearman).

##Set up gearman environment

For Ubuntu/Debian OS (apt-get install):

```
# install gearman server
$ aptitude -y install gearman gearman-job-server libgearman-dev libdrizzle0

# install gearman extension
$ pecl install channel://pecl.php.net/gearman-1.1.2
```
