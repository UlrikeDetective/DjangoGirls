# Deploying the Comment Feature to PythonAnywhere

Follow these steps to get your new comment system live on your PythonAnywhere website.

---

### Step 1: Commit and Push Your Code

First, you need to save all your local changes to your Git repository and push them to your remote provider (like GitHub).

1.  Open your local terminal or command prompt.
2.  Add all your changed files to the staging area:
    ```bash
    git add .
    ```
3.  Commit the changes with a descriptive message:
    ```bash
    git commit -m "Implement blog comment feature"
    ```
4.  Push the commit to your remote repository:
    ```bash
    git push
    ```

---

### Step 2: Pull Changes on PythonAnywhere

Now, you need to update the code on the PythonAnywhere server.

1.  Log in to your [PythonAnywhere account](https://www.pythonanywhere.com/).
2.  Go to the **"Consoles"** tab and open a **Bash** console.
3.  In the console, navigate to your project directory if you're not already there. It should be something like `~/your-username.pythonanywhere.com`.
4.  Pull the latest changes from your repository:
    ```bash
    git pull
    ```
    You should see a summary of the files that were updated.

---

### Step 3: Run Database Migrations

Since you added a new `Comment` model, you must update your live database on PythonAnywhere.

1.  While still in the **Bash console** on PythonAnywhere, run the migrate command:
    ```bash
    python manage.py migrate
    ```
    This will apply the new `comment` table to your production database.

---

### Step 4: Set Up Environment Variables (for Email Alerts)

If you implemented the optional email alert feature, you must securely set your email credentials on PythonAnywhere. **Do not hardcode them in your `settings.py` file.**

1.  Go to the **"Web"** tab on your PythonAnywhere dashboard.
2.  Scroll down to the **"Code"** section.
3.  Find the **"Environment variables"** section.
4.  Add your email credentials as two separate variables:
    *   **Variable Name:** `EMAIL_HOST_USER`, **Value:** `your-email@gmail.com`
    *   **Variable Name:** `EMAIL_HOST_PASSWORD`, **Value:** `your-16-character-app-password`
5.  Click the **"Add"** button for each one.

---

### Step 5: Reload Your Web App

The final and most important step is to reload your web application so that PythonAnywhere starts using your new code.

1.  Stay on the **"Web"** tab.
2.  Click the big green **"Reload your-username.pythonanywhere.com"** button at the top of the page.
3.  Wait a few moments for the reloading process to complete.

---

That's it! Your comment feature should now be live on your website. You can test it by visiting a blog post and adding a new comment. Remember to check the Django admin panel to moderate comments if you have that feature enabled.