# Flask Todo App

This is a simple Flask Todo application that demonstrates CRUD operations using Flask and SQLAlchemy. The project consists of endpoints to create, update, and delete todo items.

## Endpoints Overview

- **`/` (Homepage)**
  - **Methods:** `GET`, `POST`
  - **Description:** 
    - `GET`: Renders the homepage and displays all todo items.
    - `POST`: Receives form data (`title` and `desc`) to create a new todo item, then adds it to the database.
   
      ```bash
      @app.route('/', methods=['GET', 'POST'])
      def homepage():
          if request.method == "POST":
              print("post")
              title = request.form['title']
              # print(title)
              desc = request.form['desc']
              todo = Todo(title=title, desc=desc)
              db.session.add(todo)
              db.session.commit()
          todos = Todo.query.all()
          return render_template('index.html', todos=todos)
      ```

- **`/update/<int:sno>`**
  - **Methods:** `GET`, `POST`
  - **Description:** 
    - `GET`: Retrieves the todo item identified by its serial number (`sno`) and renders a template for updating.
    - `POST`: Updates the todo item with new `title` and `desc` from the form and commits the changes.
      
  ```bash
  @app.route('/update/<int:sno>', methods=['GET', 'POST'])
  def update(sno):
      if request.method == "POST":
          title = request.form['title']
          desc = request.form['desc']
          todo = Todo.query.filter_by(sno=sno).first()
          todo.title = title
          todo.desc = desc
          db.session.commit()
          return redirect("/")
  
      todo = Todo.query.filter_by(sno=sno).first()
      return render_template('update.html', todo=todo)
  ```

- **`/delete/<int:sno>`**
  - **Methods:** `GET`, `POST`
  - **Description:** 
    - Deletes the todo item identified by its serial number (`sno`) from the database and redirects back to the homepage.
   
  ```bash
  @app.route('/delete/<int:sno>', methods=['GET', 'POST'])
  def delete(sno):
      todo = Todo.query.filter_by(sno=sno).first()
      if todo:
          db.session.delete(todo)
          db.session.commit()
      return redirect("/")
  ```

## Setup & Installation

### 1. Clone the Repository
```bash
git clone https://github.com/Somgester/flask-todo.git
cd flask-todo

```
### 2. Install Flask and other dependencies
  ```bash
pip install flask

```
```bash
pip install datetime

```
```bash
pip install virtualenv

```

```bash
pip install flask-sqlalchemy 

```

   

### 3. Create virtual environment
   ```bash
    virtualenv env  
   ```

### 4. Activate the virtual environment
  - Windows
       ```bash
       .\env\Scripts\activate.ps1 
       ```
  - MacOs/Linux
    ```bash
    source env/bin/activate
    ```

# Additional Notes

- **Database:** The app uses SQLite. The database file (`todo.db`) will be created by the following commands in folder named instance.
  ```bash
  python
  
  from app import app, db
  with app.app_context():
    db.create_all()
  ```

- **Templates:** Make sure you have the corresponding HTML templates (e.g., `index.html`, `update.html`) in your `templates` folder.

- **Time Zone:** The `date_created` field in the `Todo` model uses the current UTC time.

