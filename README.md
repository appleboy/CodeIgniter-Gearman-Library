#Codeigniter Gearman Library

This is a library to use gearman within codeigniter. The original source was pulled from Sunil here https://github.com/sunils34/codeigniter-gearman-library. I add some feature and remove deprecated function.

Please make sure you have setuped gearman daemon environment and gearman pecl drivers. (http://pecl.php.net/package/gearman).

##Set up gearman environment

For Ubuntu/Debian OS (apt-get install):

install gearman server

```
$ aptitude -y install gearman gearman-job-server libgearman-dev libdrizzle0
```

install gearman extension

```
$ pecl install channel://pecl.php.net/gearman-1.1.2
```

## Installation

You can install via http://getsparks.org/packages/codeigniter-gearman/show

```
$ php tools/spark install -v1.0.2 codeigniter-gearman
```

or manual install

```
$ cp config/gearman.php your_application/config/
$ cp libraries/lib_gearman.php your_application/libraries/
```

## Usage

load library from spark:

```
$this->load->spark('codeigniter-gearman/1.0.2');
```

or load library from manual install:

```
$this->load->library('lib_gearman');
```

### Client Example

Please refer: `example/cli.php`

    public function client()
    {
        $this->lib_gearman->gearman_client();

        $emailData = array(
            'name'  => 'web',
            'email' => 'member@example.com',
        );
        $imageData = array(
            'image' => '/var/www/pub/image/test.png',
        );

        $this->lib_gearman->do_job_background('sendEmail', serialize($emailData));
        echo "Email sending is done.\n";
        $this->lib_gearman->do_job_background('resizeImage', serialize($imageData));
        echo "Image resizing is done.\n";
    }

### Worker example

    public function worker()
    {
        $worker = $this->lib_gearman->gearman_worker();

        $this->lib_gearman->add_worker_function('sendEmail', 'Cli::doSendEmail');
        $this->lib_gearman->add_worker_function('resizeImage', 'Cli::doResizeImage');

        while ($this->lib_gearman->work()) {
            if (!$worker->returnCode()) {
                echo "worker done successfully \n";
            }
            if ($worker->returnCode() != GEARMAN_SUCCESS) {
                echo "return_code: " . $this->lib_gearman->current('worker')->returnCode() . "\n";
                break;
            }
        }
    }

### Define job function

    public static function doSendEmail($job)
    {
        $data = unserialize($job->workload());
        print_r($data);
        sleep(2);
        echo "Email sending is done really.\n\n";
    }

    public static function doResizeImage($job)
    {
        $data = unserialize($job->workload());
        print_r($data);
        sleep(2);
        echo "Image resizing is really done.\n\n";
    }

### Run Test

run worker:

```
$ php app/index.php cli worker
```

run client:

```
$ php app/index.php cli client
$ php app/index.php cli client
$ php app/index.php cli client
....
```
