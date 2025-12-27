# Django Project Instructions

## 1. Local Development Setup

**Activate Virtual Environment:**
```bash
source venv/bin/activate
```

**Run Development Server:**
```bash
python manage.py runserver
```

## 2. Local Access

*   **Homepage:** [http://127.0.0.1:8000/](http://127.0.0.1:8000/)
*   **Admin Login:** [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)
    *   *Credentials:* See `.env` file for username and password.

## 3. Content Management

**Create New Post:**
[http://127.0.0.1:8000/post/new/](http://127.0.0.1:8000/post/new/)

**Approve or Remove Comments:**
*   **Via Admin Panel:** [http://127.0.0.1:8000/admin/blog/comment/](http://127.0.0.1:8000/admin/blog/comment/)
*   **Via Post Page:**
    1.  Log in as admin. [https://patterndisrupt.pythonanywhere.com/admin](https://patterndisrupt.pythonanywhere.com/admin)
    2.  Navigate to the post on the website.
    3.  Use the "Approve" or "Remove" buttons displayed next to each comment.

## 4. Git Workflow

To save changes and push to the repository:
```bash
git add .
git commit -m "Update website"
git push
```

## 5. Deployment (PythonAnywhere)

**Live Website:** [https://patterndisrupt.pythonanywhere.com](https://patterndisrupt.pythonanywhere.com)

**Update Procedure:**
1.  Log in to [PythonAnywhere Dashboard](https://www.pythonanywhere.com/).
2.  Open a **Bash Console**.
3.  Navigate to your project directory:
    ```bash
    cd patterndisrupt.pythonanywhere.com
    ```
    *(Use `ls` to confirm the folder name if unsure)*
4.  Pull the latest changes from GitHub:
    ```bash
    git pull
    ```
5.  **Update Static Files (if CSS/images changed):**
    ```bash
    python manage.py collectstatic --noinput
    ```
6.  **Reload Web App:**
    *   Go to the **Web** tab on the PythonAnywhere dashboard.
    *   Click the green **Reload** button.
