# Ansible playbook for headless Chrome with ChromeDriver  

This playbook will download and install the latest version of Chrome and ChromeDriver.  

The latest version of ChromeDriver allows you to run Chrome browser in headless mode without Selenium and doesn't need any Display server or Xvfb.

## Requirements

- git
- ansible
- codeception _(for tests)_

## Setup

1. Clone this repo with git  

```shell
    $ git clone https://github.com/marvin-neumann/ansible-chrome-headless.git
```

2. Then run the playbook with ansible-playbook  

```shell
    $ ansible-playbook -l localhost ansible-chrome-headless/playbook.yml --ask-sudo-pass
```

## Codeception Acceptance testing with Chrome headless  

1. Change the `acceptance.suite.yml` in your tests directory, put everything below in the `Webdriver` part und change the url.  

```yaml
        WebDriver:
            url: 'http://example.com/'
            browser: chrome
            port: 9515
            window_size: false
            capabilities:
                chromeOptions:
                    args: ["--headless", "--disable-gpu", "window-size=1920x1080"]
                    binary: "/usr/bin/google-chrome"
```

2. Start ChromeDriver, the binary is placed in `/usr/local/bin` so you can call it from anywhere  

```shell
    $ chromedriver --url-base=/wd/hub --whitelisted-ips='' --verbose
```

3. Then run your tests.  

## Acceptance test issues with ChromeDriver v2.31  
Even though you are writing valid tests, there are a few problems that can occur when running some versions of ChromeDriver.  

- If an element cannot be clicked because it is outside the viewport, you have to scroll to the element first, then click the element.  

```php
    $I->scrollTo('button.outside-viewport');
    $I->click(['css' => 'button.outside-viewport']);;
```

- If filling a date input field with the `fillField` action doesn't work, use the `pressKey` action instead.

```php
    # if this doesn't work  
    $I->fillField('input#date', '2017-08-31');  
```

```php
    # use this instead  
    $I->pressKey('input#date', '2017-08-31');  
```
