# Instructions for Adding a Commenting System to Your Django Blog

Here is a step-by-step guide to implement a simple, self-hosted commenting system for your blog application.

---

### Step 1: Create the Comment Model

First, you need to define what a "comment" is in your database.

1.  Open `blog/models.py`.
2.  Add the following `Comment` model class at the end of the file. This will associate comments with posts and include fields for the author, text, creation date, and an approval status for moderation.

    ```python
    from django.db import models
    from django.utils import timezone

    # (Your existing Post model should be here)

    class Comment(models.Model):
        post = models.ForeignKey('blog.Post', on_delete=models.CASCADE, related_name='comments')
        author = models.CharField(max_length=200)
        text = models.TextField(max_length=500)
        created_date = models.DateTimeField(default=timezone.now)
        approved_comment = models.BooleanField(default=False)

        def approve(self):
            self.approved_comment = True
            self.save()

        def __str__(self):
            return self.text
    ```

---

### Step 2: Create and Apply Database Migrations

After defining the new model, you need to update your database schema.

1.  Open your terminal or command prompt.
2.  Run the following commands one by one:

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

---

### Step 3: Create a Form for Comments

Next, create a form that users will fill out to leave a comment.

1.  Open `blog/forms.py`.
2.  Add the `CommentForm` class to the file:

    ```python
    from django import forms
    from .models import Post, Comment

    # (Your existing PostForm should be here)

    class CommentForm(forms.ModelForm):
        class Meta:
            model = Comment
            fields = ('author', 'text',)
    ```

---

### Step 4: Update the View to Handle Comments

Now, you need to modify the `post_detail` view to handle displaying the comment form and saving new comments.

1.  Open `blog/views.py`.
2.  Import the `CommentForm` and `Comment` model at the top.
3.  Find the `post_detail` view and update it to match the following code. This adds logic to process the form and pass the comment list to the template.

    ```python
    from django.shortcuts import render, get_object_or_404, redirect
    from .models import Post, Comment # Make sure to import Comment
    from .forms import PostForm, CommentForm # Make sure to import CommentForm

    # (Other views like post_list, post_new, etc. are here)

    def post_detail(request, pk):
        post = get_object_or_404(Post, pk=pk)
        if request.method == "POST":
            form = CommentForm(request.POST)
            if form.is_valid():
                comment = form.save(commit=False)
                comment.post = post
                comment.save()
                # Optional: Add your email alert logic here
                return redirect('post_detail', pk=post.pk)
        else:
            form = CommentForm()
        return render(request, 'blog/post_detail.html', {'post': post, 'form': form})
    ```

---

### Step 5: Update the Template to Display Comments and Form

The final step for displaying comments is to update the post detail template.

1.  Open `blog/templates/blog/post_detail.html`.
2.  Add the following code right below the post's content, likely before the closing `</div>` or `</article>` tag. This will display existing comments and the form to add a new one.

    ```html
    {# (existing post content) ... right after the post's text/image #}

    <hr>
    <h2>Comments</h2>

    {% for comment in post.comments.all %}
        {% if comment.approved_comment %}
            <div class="comment">
                <div class="date">{{ comment.created_date }}</div>
                <strong>{{ comment.author }}</strong>
                <p>{{ comment.text|linebreaks }}</p>
            </div>
        {% endif %}
    {% empty %}
        <p>No comments here yet :(</p>
    {% endfor %}

    <hr>

    <h3>Add a new comment</h3>
    <form method="POST" class="post-form">{% csrf_token %}
        {{ form.as_p }}
        <button type="submit" class="save btn btn-secondary">Send</button>
    </form>
    ```

---

### Step 6: Enable Comment Moderation in the Admin

To allow yourself to approve or delete comments, register the `Comment` model in the Django admin.

1.  Open `blog/admin.py`.
2.  Add the following code:

    ```python
    from django.contrib import admin
    from .models import Post, Comment # Make sure to import Comment

    admin.site.register(Post)
    admin.site.register(Comment) # Register the Comment model
    ```

Now you can log into your admin site (`/admin`) and you will see a "Comments" section where you can approve new comments by checking the "Approved comment" box.

---

### Step 7: (Optional) Set Up Email Alerts

To get an email when a comment is posted, you'll need to configure Django's email settings.

1.  Open `mysite/settings.py`.
2.  Add the following settings at the end of the file, filling in your email provider's details. **Remember to use environment variables for sensitive information like passwords.**

.env
    ```bash
        EMAIL_HOST_USER=your-email@example.com
        EMAIL_HOST_PASSWORD=your-email-password-or-app-password
    ```

    ```python
    # Email settings for comment notifications
    EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
    EMAIL_HOST = 'smtp.gmail.com'
    EMAIL_PORT = 587
    EMAIL_USE_TLS = True

    # Retrieve values from the environment
    EMAIL_HOST_USER = os.getenv('EMAIL_HOST_USER')
    EMAIL_HOST_PASSWORD = os.getenv('EMAIL_HOST_PASSWORD')
    ```

3.  Then, in `blog/views.py`, update the `post_detail` view to send an email:

    ```python
    # At the top of blog/views.py
    from django.core.mail import send_mail

    # Inside the post_detail view, after comment.save()
    if form.is_valid():
        comment = form.save(commit=False)
        comment.post = post
        comment.save()

        # Send email alert
        send_mail(
            'New Comment on Your Blog Post',
            f'A new comment was posted on "{post.title}" by {comment.author}.\n\nRead it here: {request.build_absolute_uri()}',
            'from-email@example.com', # Should match EMAIL_HOST_USER
            ['admin-email@example.com'], # Your email
            fail_silently=False,
        )

        return redirect('post_detail', pk=post.pk)
    ```

This completes the setup. When you are ready to continue, follow these steps in order.

---

### Step 8 (Optional): How to Disable Comment Moderation

If you prefer comments to appear on your blog immediately without needing your approval, you can disable the moderation feature.

This involves two small changes: one in the model to approve comments by default, and one in the template to remove the check.

1.  **Update the Comment Model**

    -   Open `blog/models.py`.
    -   Find the `Comment` model.
    -   Change the `default` value of the `approved_comment` field from `False` to `True`.

    **Change this:**
    ```python
    approved_comment = models.BooleanField(default=False)
    ```

    **To this:**
    ```python
    approved_comment = models.BooleanField(default=True)
    ```

2.  **Apply the Database Change**

    -   Because you've changed a model's default value, you need to update the database. Run the following commands:
    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

3.  **Update the Template**

    -   Now that all comments are approved by default, you can remove the approval check from the template.
    -   Open `blog/templates/blog/post_detail.html`.
    -   Find the loop that displays comments and remove the `{% if comment.approved_comment %}` and `{% endif %}` tags surrounding the comment `<div>`.

    **Change this:**
    ```html
    {% for comment in post.comments.all %}
        {% if comment.approved_comment %}
            <div class="comment">
                <div class="date">{{ comment.created_date }}</div>
                <strong>{{ comment.author }}</strong>
                <p>{{ comment.text|linebreaks }}</p>
            </div>
        {% endif %}
    {% empty %}
        <p>No comments here yet :(</p>
    {% endfor %}
    ```

    **To this:**
    ```html
    {% for comment in post.comments.all %}
        <div class="comment">
            <div class="date">{{ comment.created_date }}</div>
            <strong>{{ comment.author }}</strong>
            <p>{{ comment.text|linebreaks }}</p>
        </div>
    {% empty %}
        <p>No comments here yet :(</p>
    {% endfor %}
    ```

With these changes, new comments will be visible on your posts as soon as they are submitted. You will still be able to un-approve or delete them from the admin panel if you need to.
