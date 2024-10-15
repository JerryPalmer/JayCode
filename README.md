from flask import Flask, render_template, request, redirect, url_for, flash
from flask_mail import Mail, Message

app = Flask(__name__)
app.secret_key = 'your_secret_key'  # Replace with a secure secret key

# Flask-Mail Configuration
app.config['MAIL_SERVER'] = 'smtp.gmail.com'
app.config['MAIL_PORT'] = 587
app.config['MAIL_USE_TLS'] = True
app.config['MAIL_USERNAME'] = 'your_email@gmail.com'  # Replace with your email
app.config['MAIL_PASSWORD'] = 'your_email_password'   # Replace with your email password or app password

mail = Mail(app)

@app.route('/')
def home():
    band_name = "Candy Vibe Music"
    return render_template('index.html', band_name=band_name)

@app.route('/about')
def about():
    band_name = "Candy Vibe Music"
    return render_template('about.html', band_name=band_name)

@app.route('/contact', methods=['GET', 'POST'])
def contact():
    band_name = "Candy Vibe Music"
    if request.method == 'POST':
        name = request.form.get('name')
        email = request.form.get('email')
        message = request.form.get('message')

        # Create email message
        msg = Message(f'New Contact Form Submission from {name}',
                      sender=email,
                      recipients=['palmerk'])  # Replace with your email

        msg.body = f'''
        From: {name} <{email}>
        Message: {message}
        '''
        try:
            mail.send(msg)
            flash('Message sent successfully!', 'success')
            return redirect(url_for('contact'))
        except Exception as e:
            print(e)
            flash('Failed to send message. Please try again.', 'danger')
            return redirect(url_for('contact'))
    
    return render_template('contact.html', band_name=band_name)

if __name__ == '__main__':
    app.run(debug=True)
