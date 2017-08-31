# Ansible playbook for ChromeDriver and Chrome

This playbook will download and install the latest 
versions of ChromeDriver and Google Chrome browser.

This is all you need to run Chrome in headless mode for acceptance testing.

## Codeception Acceptance testing with Chrome headless
1. Configure `acceptance.suite.yml`
  
        WebDriver:
            url: 'http://ecample.com/'
            browser: chrome
            port: 9515
            window_size: false
            capabilities:
                chromeOptions:
                    args: ["--headless", "--disable-gpu", "window-size=1920x1080"]
                    binary: "/usr/bin/google-chrome"

2. Start ChromeDriver  
`$ chromedriver --url-base=/wd/hub --whitelisted-ips='' --verbose`

3. Then run your tests.  

## Acceptance test issues with Chrome

- Sometimes the element cannot be clicked because it is not inside the viewport.  
   You have to scroll to the element first, then click the element.
```
   $I->scrollTo('button.outside-viewport');
   $I->click(['css' => 'button.outside-viewport']);;
```
- Date inputs fail with the `fillField` action, use the `pressKey` action instead.
```
    # doesn't work  
    $I->fillField('#date', '2017-08-31);  
    
    # use this instead  
    $I->pressKey('#date', '2017-08-31);  
```