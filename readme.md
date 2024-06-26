for creating a virtual environment for this repo :

for creation -> python3 -m venv .venv
for activation -> source .venv/bin/activate
for demounting the venv -> deactivate

once the venv is mounted , install the Flask package :

pip install Flask

after Flask is installed , start the server with :

Running flask locally --->>>>>> flask --app hello run (for development server with fastapi watching for changes)

For production :
 As the log message says, we need another way to use a WSGI server for going to production. So, let's do it.

WSGI
WSGI stands for Web Server Gateway Interface. It is a specification for a standardized interface between web servers and web applications or frameworks written in Python. WSGI defines a contract that allows web servers to communicate with Python web applications, enabling them to work together seamlessly.

This separation of concerns allows web servers to focus on handling low-level networking tasks, such as accepting incoming requests and managing connections. In contrast, web applications can focus on handling the application logic and generating responses.

You can switch between server implementations with minimal application changes. In this case, we'll use Gunicorn.

First, we configure the Gunicorn server by creating a new file named gunicorn_config.py, where we'll configure things like the number of workers and threads and open port or timeout.

import os



workers = int(os.environ.get('GUNICORN_PROCESSES', '2'))

threads = int(os.environ.get('GUNICORN_THREADS', '4'))

# timeout = int(os.environ.get('GUNICORN_TIMEOUT', '120'))

bind = os.environ.get('GUNICORN_BIND', '0.0.0.0:8080')



forwarded_allow_ips = '*'

secure_scheme_headers = { 'X-Forwarded-Proto': 'https' }
Copy snippet
Now, the last thing to do is start the Gunicorn server with the application deployed.

Let's run the following command in the terminal:

gunicorn --config gunicorn_config.py app:app
Copy snippet
The first app is the name of the Python file containing the Flask application.

The output shows the application up and running in Gunicorn
