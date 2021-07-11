# coffee-and-wifi
# Problem: Using WTF and forms to add a cafe nearby
# Solutions: 
1. Add a CafeForm class
```
class CafeForm(FlaskForm):
    cafe = StringField('Cafe name', validators=[DataRequired()])
    url = StringField('Cafe Location on Google Maps (URL)', validators=[DataRequired()])
    open = StringField('Opening Time e.g. 8 AM', validators=[DataRequired()])
    close = StringField('Cloing Time e.g. 11 PM', validators=[DataRequired()])
    coffee = SelectField('Coffee Rating', choices=['☕', '☕☕', '☕☕☕', '☕☕☕☕', '☕☕☕☕☕'], validators=[DataRequired()])
    wifi = SelectField('Wifi Rating', choices=['💪', '💪💪', '💪💪💪', '💪💪💪💪', '💪💪💪💪💪'], validators=[DataRequired()])
    power = SelectField('Power Rating', choices=['🔌', '🔌🔌', '🔌🔌🔌', '🔌🔌🔌🔌', '🔌🔌🔌🔌🔌'],
                       validators=[DataRequired()])

    submit = SubmitField('Submit')
```
2. Make the form write a new row into cafe-data.csv
```
@app.route('/add', methods=['GET','POST'])
def add_cafe():
    form = CafeForm()
    if form.validate_on_submit():
        with open('cafe-data.csv', 'a') as csv_file:
            csv_file.write(f"\n{form.cafe.data}, {form.url.data}, {form.open.data}, {form.close.data}, {form.coffee.data}, {form.wifi.data}, {form.power.data}")
    # Exercise:
    # Make the form write a new row into cafe-data.csv
    # with   if form.validate_on_submit()
    return render_template('add.html', form=form)
```
3. Show cafes added from the forms
```
@app.route('/cafes')
def cafes():
    with open('cafe-data.csv', newline='') as csv_file:
        csv_data = csv.reader(csv_file, delimiter=',')
        list_of_rows = []
        for row in csv_data:
            list_of_rows.append(row)
    return render_template('cafes.html', cafes=list_of_rows)
```
