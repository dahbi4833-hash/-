from flask import Flask, request, redirect, url_for, render_template

app = Flask(__name__)

# بيانات المستخدمين الوهمية
users = {}

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        if username in users and users[username] == password:
            return redirect(url_for('profile', username=username))
        else:
            return 'اسم المستخدم أو كلمة المرور غير صحيحة'
    return render_template('login.html')

@app.route('/register', methods=['GET', 'POST'])
def register():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        users[username] = password
        return redirect(url_for('login'))
    return render_template('register.html')

@app.route('/profile/<username>')
def profile(username):
    return render_template('profile.html', username=username)

if __name__ == '__main__':
    app.run(debug=True)
