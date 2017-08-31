# Ansible playbook for ChromeDriver and Chrome

This playbook will download and install the latest 
versions of ChromeDriver and Google Chrome browser.

This is all you need to run Chrome in headless mode for testing.

## Acceptance testing with chrome headless
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
3. Run acceptance tests