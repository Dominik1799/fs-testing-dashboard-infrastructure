# fs-testing-dashboard

fs-testing-dashboard => backend
fs-testing-dashboard-client => frontend
fs-testing-dashboard-infrastructure => infrastructure files. Docker compose containing setup + nginx config file for reverse proxy


## About
Basic premise of this app is to simplify automatic testing process in forumSTAR project. The whole process works something like this:

 1. QFTEST runs a bunch of tests on a windows server machine
 2. After it finishes testing forumSTAR app, it calls a custom script that uploads resulting test reports to a machine (using WinSCP CLI interface) that has fs-testing-dashboard running
 3. After uploading these files, the same script makes a HTTP call to fs-testing-dashboard API endpoint (backend) to indicate that it should index new reports.
 4. fs-testing-dashboard indexes these reports inside MongoDB. It uses HTML parsing as these testing reports are simply HTML files and extracts important info.
 5. These new test result reports are available via the web UI contained inside fs-testing-dashboard-client.
 

The backend and frontend both use Python Flask library for everything HTTP related. Frontend uses Jinja2 markup to generate HTML pages - nothing too complicated, basically just a step above plain HTML. For styling, bootstrap library is used, I think I did not use a single line of my CSS.

It uses NGINX for reverse proxy routing. Backend (API) is available under /api subroute, please check the app.conf to view NGINX settings.

Everything is dockerized, inside this docker-compose is also mongo-express, which technically is not needed, it is only used for debugging so feel free to delete this service.

Every component is configurable using ENV variables, so when developing localy, create a ".env" file (yes, only .env without filename) in the root directory of a component you are developing and the variables inside this file will be available during the runtime of the program.

## What it needs
This app is VERY unfinished. It should atleast have paging mechanism for browsing stored records, as it currently only shows the last 10 testing days. Anything older cannot be viewed using this application.

This app could be the perfect way to display various data visualizations from testing. Simply parse the uploaded records using a HTML parser and pull out relevant data. Corporate customers like colorful graphs :)
