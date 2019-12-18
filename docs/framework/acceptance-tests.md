# Acceptance Tests

A ready-to-use [TestCafe](https://devexpress.github.io/testcafe/) test suite for acceptance tests is located in `frontend/tests/acceptance/`:

    /var/www/html# make test-acceptance
    Running acceptance tests in Chromium headless...
    
    > symlex-frontend@0.0.0 test-chromium /var/www/html/frontend
    > testcafe "chromium:headless --disable-dev-shm-usage" --skip-js-errors --selector-timeout 5000 -S -s tests/screenshots tests/acceptance --reporter spec
    
     Running tests in:
     - HeadlessChrome 73.0.3683 / Linux 0.0.0
    
     change password page
     ✓ View change password page
    
     edit profile page
     ✓ View edit profile page
    
     homepage
     ✓ View Homepage
    
     login
     ✓ Login
    
     logout
     ✓ Logout

Note that you need to have TestCafe installed locally to run this outside the Docker container.
The main config file is `frontend/tests/acceptance/testcafeconfig.json`.