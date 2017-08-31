# Ansible playbook for headless Chrome and ChromeDriver in Ubuntu 16.04 LTS

This playbook will download and install the latest version of Chrome and ChromeDriver in Ubuntu 16.04 LTS.  

The latest version of ChromeDriver allows you to run Chrome browser in headless mode without Selenium and doesn't need any Display server or Xvfb. 

##  How to install this playbook

1. Clone this repo with git 

```
    $ git clone https://github.com/marvin-neumann/ansible-chrome-headless.git
```

2. Then run the playbook with ansible-playbook

```
    $ ansible-playbook -l localhost ansible-chrome-headless/playbook.yml --ask-sudo-pass
```

## Codeception Acceptance testing with Chrome headless
1. Change the `acceptance.suite.yml` in your tests directory, put everything below in the `Webdriver` part und change the url.

```
        WebDriver:
            url: 'http://ecample.com/'
            browser: chrome
            port: 9515
            window_size: false
            capabilities:
                chromeOptions:
                    args: ["--headless", "--disable-gpu", "window-size=1920x1080"]
                    binary: "/usr/bin/google-chrome"
```

2. Start ChromeDriver, the binary is placed in /usr/bin so you can call it from anywhere  

```
    $ chromedriver --url-base=/wd/hub --whitelisted-ips='' --verbose
```

3. Then run your tests.  

## Acceptance test issues with Chrome

- If an element cannot be clicked because it is outside the viewport, you have to scroll to the element first, then click the element.

```
    $I->scrollTo('button.outside-viewport');
    $I->click(['css' => 'button.outside-viewport']);;
```

- Date input fields fail with the `fillField` action, use the `pressKey` action instead.

```
    # doesn't work  
    $I->fillField('input#date', '2017-08-31);  
    
    # use this instead  
    $I->pressKey('input#date', '2017-08-31);  
```
