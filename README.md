## 🚀 Learning Django: Building My First Projects with Python's Web Framework

After diving into Django, I created a couple of beginner-friendly projects that helped me grasp the basics of web development. Here, I'm sharing two simple Django projects that demonstrate the power of this Python framework for handling user input, managing data, and implementing user authentication.

For those interested in **Django basics and commands**, check out this [Quick Start Guide on LinkedIn](https://www.linkedin.com/posts/saityourname/django-basics) for essential commands, like:


1. **Starting a new Django project**:
    ```bash
    django-admin startproject projectname
    ```

2. **Creating a new app within a project**:
    ```bash
    python manage.py startapp appname
    ```

3. **Applying migrations** (for database setup and model changes):
    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

4. **Running the development server**:
    ```bash
    python manage.py runserver
    ```

5. **Creating a superuser** (for admin access):
    ```bash
    python manage.py createsuperuser
    ```

For a detailed list of these commands, visit [this LinkedIn post on Django commands](https://www.linkedin.com/posts/saityourname/django-commands).

---

### 🛠 Project 1: Simple Website with Contact Form

This project includes a homepage and a contact form where users can submit their details. The data is saved in the database.

#### Steps:
1. **Create a new Django app** (e.g., `website`) and add it to `INSTALLED_APPS` in `settings.py`.
2. **Set up URLs** in `urls.py`.
3. **Define Models**, **Forms**, **Views**, and **Templates**.

#### Code:

1. **`models.py`**: Define the Contact model to store user input.

    ```python
    from django.db import models

    class Contact(models.Model):
        name = models.CharField(max_length=100)
        email = models.EmailField()
        message = models.TextField()

        def __str__(self):
            return self.name
    ```

2. **`forms.py`**: Create a form for user input.

    ```python
    from django import forms
    from .models import Contact

    class ContactForm(forms.ModelForm):
        class Meta:
            model = Contact
            fields = ['name', 'email', 'message']
    ```

3. **`views.py`**: Handle the form submission.

    ```python
    from django.shortcuts import render, redirect
    from .forms import ContactForm

    def home(request):
        return render(request, 'home.html')

    def contact(request):
        if request.method == 'POST':
            form = ContactForm(request.POST)
            if form.is_valid():
                form.save()
                return redirect('home')
        else:
            form = ContactForm()
        return render(request, 'contact.html', {'form': form})
    ```

4. **`urls.py`**: Link views to URLs.

    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.home, name='home'),
        path('contact/', views.contact, name='contact'),
    ]
    ```

5. **`contact.html` Template**:

    ```html
    <h1>Contact Us</h1>
    <form method="post">
        {% csrf_token %}
        {{ form.as_p }}
        <button type="submit">Submit</button>
    </form>
    ```

---

### 🔒 Project 2: User Authentication (Login & Logout)

This project includes login and logout functionality with Django's built-in authentication system.

#### Steps:
1. **Set up URLs** in `urls.py`.
2. **Create Login and Logout Views**.
3. **Create HTML Templates** for login.

#### Code:

1. **`views.py`**: Handle login and logout.

    ```python
    from django.shortcuts import render, redirect
    from django.contrib.auth import authenticate, login, logout
    from django.contrib import messages

    def user_login(request):
        if request.method == 'POST':
            username = request.POST.get('username')
            password = request.POST.get('password')
            user = authenticate(request, username=username, password=password)
            if user is not None:
                login(request, user)
                return redirect('home')
            else:
                messages.error(request, 'Invalid username or password')
        return render(request, 'login.html')

    def user_logout(request):
        logout(request)
        return redirect('login')
    ```

2. **`urls.py`**:

    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('login/', views.user_login, name='login'),
        path('logout/', views.user_logout, name='logout'),
        path('', views.home, name='home'),  # Assume `home` view is defined
    ]
    ```

3. **`login.html` Template**:

    ```html
    <h2>Login</h2>
    <form method="post">
        {% csrf_token %}
        <label for="username">Username:</label>
        <input type="text" name="username" required>
        <label for="password">Password:</label>
        <input type="password" name="password" required>
        <button type="submit">Login</button>
    </form>
    ```

---

These examples gave me a strong foundation in Django's core functionality and helped me understand how to build secure, scalable web applications. Excited to keep learning and building! 🚀

Let me know your thoughts or questions about Django – always happy to connect and share more insights!
