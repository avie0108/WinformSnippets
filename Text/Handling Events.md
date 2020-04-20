# Handling Events

## Button Presses

When adding a button to your form using the designer tab, you can double-click the button to create a new `button_Click()` method.

The method will look something like this:

```cs
private void button1_Click(object sender, EventArgs e)
{

}
```

The `sender` parameter is our newly created button, but before we can use it as a button it must first be cast to the correct type. If we want to change the button text when clicking it, we can do the following:

```cs
private void button1_Click(object sender, EventArgs e)
{
	Button button = (Button)sender;
	// Alternatively, you can define the button with 'var'
	// var button = (Button)sender;

	button.Text = "Heyoooo!";
}
```

We can modify the button to be named `Arrest` by setting the `Designer -> (Name)` value and make it display a message by using the `MessageBox.Show()` method as follows:

```cs
private void Arrest_Click(object sender, EventArgs e)
{
	MessageBox.Show("Stop! In the name of the Jarl!");
}
```

Renaming the button after you made the `_Click` method will not rename said method. Instead you need to "refactor" the method yourself. Visual Studio has a refactoring shortcut mapped to `Ctrl+R, Ctrl+R

![refactor](Assets/Handling%20Events/refactoring.png)
