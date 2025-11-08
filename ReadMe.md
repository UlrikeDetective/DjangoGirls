# Django Girls Blog

This is a Django-based blog application.

## First-Time Setup

To run this application for the first time, follow these steps:

1.  **Create and activate a virtual environment:**
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Apply database migrations:**
    ```bash
    python manage.py migrate
    ```

4.  **Create a superuser:**
    This will create an admin account.
    ```bash
    python manage.py createsuperuser
    ```

5.  **Run the development server:**
    ```bash
    python manage.py runserver
    ```
    The application will be available at [http://127.0.0.1:8000](http://127.0.0.1:8000).

## Development

For subsequent development or running the application:

1.  **Activate the virtual environment:**
    ```bash
    source venv/bin/activate
    ```

2.  **Run the development server:**
    ```bash
    python manage.py runserver
    ```

3.  **When making changes to static files (CSS, JS, images):**
    Run this command to collect static files.
    ```bash
    python manage.py collectstatic
    ```

## Deployment on PythonAnywhere
- Push to github

### Initial Deployment

1.  **Auto-configure Django project:**
    This command will set up your Django project on PythonAnywhere. Replace the git repository URL with your own.
    ```bash
    pa_autoconfigure_django.py --python=3.10 --nuke https://github.com/UlrikeDetective/DjangoGirls.git
    ```

    Go to https://www.pythonanywhere.com/

2.  **Activate your virtual environment:**
    Replace `patterndisrupt.pythonanywhere.com` with your site name.
    ```bash
    workon patterndisrupt.pythonanywhere.com
    ```

3.  **Install any new dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Collect static files:**
    ```bash
    python manage.py collectstatic
    ```

### Updating the Website

git push

1.  **Open a console on PythonAnywhere.**
    pythonanywhere.com
2.  **Navigate to your project directory:**
    Replace `patterndisrupt.pythonanywhere.com` with your directory name.
    ```bash
    cd patterndisrupt.pythonanywhere.com
    ```

3.  **Pull the latest changes from your git repository:**
    ```bash
    git pull
    ```

4.  **Activate your virtual environment:**
    Replace `patterndisrupt.pythonanywhere.com` with your site name.
    ```bash
    workon patterndisrupt.pythonanywhere.com
    ```

5.  **Install any new dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

6.  **Collect static files:**
    ```bash
    python manage.py collectstatic
    ```
7.  **Reload your web app** from the PythonAnywhere dashboard.