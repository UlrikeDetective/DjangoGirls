# Project Overview

This project is a Django-based blog application. It allows users to create, view, and edit blog posts.

# Changes Made

The following changes were made to implement image uploads for blog posts:

*   **Model:** Added an `ImageField` to the `Post` model in `blog/models.py` to store uploaded images.
*   **Database:**
    *   Installed the `Pillow` library to handle image processing.
    *   Created and applied database migrations to add the new `image` column to the `blog_post` table.
*   **Forms:** Updated `blog/forms.py` to include the `image` field in the `PostForm`.
*   **Templates:**
    *   Modified `blog/templates/blog/post_detail.html` to display the uploaded image.
    *   Updated `blog/templates/blog/post_edit.html` to support file uploads by adding `enctype="multipart/form-data"` to the form.
    *   Added a link to the edit page in `blog/templates/blog/post_detail.html`.
*   **Settings:** Configured `mysite/settings.py` to handle media files by adding `MEDIA_URL` and `MEDIA_ROOT`.
*   **URLs:**
    *   Updated `mysite/urls.py` to serve media files during development.
    *   Added a URL pattern in `blog/urls.py` for editing posts.
*   **Views:**
    *   Modified the `post_new` and created the `post_edit` views in `blog/views.py` to handle image uploads (`request.FILES`).
