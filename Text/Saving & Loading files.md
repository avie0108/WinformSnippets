# Saving & Loading Files

During the project, you will have to load JSON files, modify them, and save them again. The following tutorials will explain how to open and save a text (including JSON) file in C#, and how to use the [OpenFileDialog](https://docs.microsoft.com/en-us/dotnet/framework/winforms/controls/openfiledialog-component-windows-forms) and [SaveFileDialog](https://docs.microsoft.com/en-us/dotnet/framework/winforms/controls/savefiledialog-component-windows-forms) components in Windows Forms to do the same.

## Loading files

The easiest way to load a file is by using the [File.ReadAllText](https://docs.microsoft.com/en-us/dotnet/api/system.io.file.readalltext?view=netframework-4.8) method. This method will load a text file in its entirety, and return it as a string. After obtaining this string, we can do whatever we want with it, such as writing it to our console window or [turning it into a JObject](https://www.newtonsoft.com/json/help/html/ParsingLINQtoJSON.htm)

In some cases, it might be necessary to use a `for` or `foreach` loop to iterate over each line in a text file. For this, we can load the file using [File.ReadAllLines](https://docs.microsoft.com/en-us/dotnet/api/system.io.file.readalllines?view=netframework-4.8). This function returns a string array that contains each line.

### File.ReadAllText Example

Assume that we have a file named `quote.txt`. This file contains the following;

```text
They asked me how well I understood theoretical physics.
I said I had a theoretical degree in physics.
They said welcome aboard.
```

We want to open this file in C# and print the contents of this file to the console. To do this, we simply have to do the following;

```cs
using System;
using System.IO;

namespace Example {
	class Program {
		static void Main() {
			// Load the file and store the contents in a variable;
			string data = File.ReadAllText("quote.txt");

			// Print the contents to the console
			Console.WriteLine(data);
		}
	}
}
```

When the above code is executed, the contents of `quote.txt` will be shown in the console window.

### File.ReadAllLines Example

Assume that we have a file named `names.txt`. This file contains the following;

```text
Gordon
Barney
Adrian
Wallace
Eli
Judith
Arne
```

We want to open this file in C#, and then print all names that start with the letter `A`. We can do this using the following code;

```cs
using System;
using System.IO;

namespace Example {
	class Program {
		static void Main() {

			// Load the file and store the contents in a variable;
			string[] data = File.ReadAllLines("names.txt");

			//Loop through each line
			foreach(var line in data) {
				
				//If the line starts with the letter A, print it.
				if (line.StartsWith('A')) {
					Console.WriteLine(line);
				}
			}
		}
	}
}
```

After running the above code, only the names `Adrian` and `Arne` will be shown in the console window.

## Saving files

Now that we can load files, we want to be able to save them as well. There are two simple ways to do this;

The first method is to use [Fire.WriteAllText](https://docs.microsoft.com/en-us/dotnet/api/system.io.file.writealltext?view=netframework-4.8#System_IO_File_WriteAllText_System_String_System_String_). This method takes two strings; the first string is the path to the file you want to write your data to, and the second string is the data you want to write to this file.

The second method is to use [File.WriteAllLines](https://docs.microsoft.com/en-us/dotnet/api/system.io.file.writealllines?view=netframework-4.8#System_IO_File_WriteAllLines_System_String_System_String___). This method takes a string containing the path to the file, and a string array where each string represents a line in the file.

__Warning: If the specified file already exists, the contents of this file will be overwritten.__ You can use the [File.Exists](https://docs.microsoft.com/en-us/dotnet/api/system.io.file.exists?view=netframework-4.8) method to check if a file already exists. It takes a file path as a string, and returns True or False depending on whether the specified file exists.

### File.WriteAllText Example

We have a function `RetrieveData()` (not shown) that retrieves some data for us and stores it in a string. This data must be written to `data.txt`. We can do this using the following code;

```cs
using System;
using System.IO;

namespace Example {
	class Program {
		static void Main() {

			// Retrieve some data;
			string data = RetrieveData();

			//Write the data to a file.
			File.WriteAllText("data.txt", data);
		}
	}
}
```

After executing the above code, the file `data.txt` will have been created, which contains the data obtained from `ReceiveData()`.

### File.WriteAllLines Example

We have the functions `RetrieveData1()`, `RetrieveData2()`, and `RetrieveData3()`, which each returns a string containing some data. We want to write this data to `data.txt`. To separate the data from each other, each string must be on its own line.

We can do this using the following code;

```cs
using System;
using System.IO;

namespace Example {
	class Program {
		static void Main() {

			// Retrieve the data;
			string[] data = new string[3];
			data[0] = RetrieveData1();
			data[1] = RetrieveData2();
			data[2] = RetrieveData3();

			//Write the data to a file.
			File.WriteAllLines("data.txt", data);
		}
	}
}
```

After executing the above code, the file `data.txt` will have been created. The first three lines of this file will contain the output of `RetrieveData1()`, `RetrieveData2()`, and `RetrieveData3()` respectively.

## Saving and Loading files with a File Dialog (Windows Forms Only)

So far, all we've done is read and write files by hard-coding the file paths into our application. But what if we want the user to tell us what to load and where to save?

In a normal console application, allowing the user to specify a file location is trivial. We can simply use [Console.ReadLine](https://docs.microsoft.com/en-us/dotnet/api/system.console.readline?view=netframework-4.8) to ask the user for a file path. But in a Windows Forms application, we have no console, and having the user enter a file path in a textbox just doesn't look good from a design perspective.

Fortunately, Windows Forms allows us to use Windows' file dialogs. We can use these dialogs to create a user-friendly way to ask users which file they want to load or where they want something to be saved. Controlling those dialogs is done through the [SaveFileDialog](https://docs.microsoft.com/en-us/dotnet/framework/winforms/controls/savefiledialog-component-windows-forms) and [OpenFileDialog](https://docs.microsoft.com/en-us/dotnet/framework/winforms/controls/openfiledialog-component-windows-forms) components.

### Using the dialogs

To setup the OpenFileDialog and SaveFileDialog components, follow these steps;

1. In Visual Studio, open your project and select the form in which the user will be asked to open a file.
2. In the Toolbox (View > Toolbox), click on "Dialogs", then double-click on either  OpenFileDialog or SaveFileDialog (depending on which you need) to add the component to this form.
3. The dialog will appear at the bottom of the designer. It's recommended that you change the `Name` property to make it easier to identify within your code. For the rest of this section we will use `Dialog1`
4. Within your code, you can now use the following snippet to open the dialog;

```cs
DialogResult = Dialog1.ShowDialog();

// We only want to continue if the user pressed on the OK button.
if(result != DialogResult.OK) {
	return;
}
```

5. After the user has selected a file to open or selected a location to save to and pressed OK, the selected filename will be contained with the `FileName` property of the dialog object.

After obtaining the path of the target file, you can load or save it as you normally would (see above)

## Additional notes

### Escape Characters

Windows uses the _backslash_ (`\`) in its file paths. Unfortunately, this character is also used by C# as an escape character, which can cause issues when working with file paths. To fix this, you have two options;

The recommended solution is to prefix your string with the `@` character (the [verbatim identifier](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/verbatim)) to tell C# that it should interpret the string exactly as it is written, and ignore whatever escape sequences it may contain. This allows you to write your file paths as you normally would. For example, `@"C:\SomeFolder\MyFile.txt"` will work fine.

Alternatively, you can replace each backslash with two backslashes (eg. using `C:\\SomeFolder\\MyFile.txt"`).

### Relative & Absolute File Paths

Up until now, you've likely specified the complete, absolute path to a file, such as `C:\ProjectA\MyFile.txt`. While this may work fine on your computer, you have no guarantee that other people will have `MyFile.txt` saved at the same location as you do. They might have it saved somewhere else, such as `D:\Documents\School\Project D\MyFile.txt`. If you code your application in such a way that `MyFile.txt` _must_ be located in `C:\ProjectA`, other people will almost certainly run into problems when using your application.

For this reason, it is highly recommended that whenever you need to read or write a file, you specify its location using a _relative file path_. This path will always be relative to your application's `.exe` file (the program's _executable_). A relative file path is any file path that doesn't start with a drive letter (or a _slash_ (`/`) for Mac OS and Linux systems). For example, `SomeFolder/MyFile.txt`.

To illustrate, pretend that you have your project saved in `C:\Project A`, and that your friend has the same project saved in `C:\Users\%username%\Documents\School\Project A`. If you were to read a file at `C:\Project A\SomeFolder\MyFile.txt`, your friend would receive an error because he does not have that file in that location. If you instead were to read the file at `SomeFolder/MyFile.txt`, then your application will read `C:\Project A\SomeFolder\MyFile.txt` on your computer and `C:\Users\%username%\Documents\School\Project A\SomeFolder\MyFile.txt` on your friend's computer.

All C# methods that accept file paths have support for relative file paths.

### Unix File Paths

If you or a member of your project team is using a Mac OS or Linux computer, you may have noticed that those devices use the regular (forward) _slash_ (`/`) in their file paths instead of the backslash that Windows uses. This difference can cause issues in your project, as Windows computers will not be able to use file paths meant for Mac OS or Linux computers and vise-versa.

To solve this, Microsoft has provided us with the [Path.Combine](https://docs.microsoft.com/en-us/dotnet/api/system.io.path.combine?view=netframework-4.8) method to create proper file paths. This method will automatically detect which operating system your computer is running in order to create a valid file path for your computer.

For example, if we were to run the following line of code;

```cs
Path.Combine("SomeFolder", "MyFile.txt")
```

On Windows, this will will return a string containing `SomeFolder\MyFile.txt`. But on Mac OS and Linux, this will return a string containing `SomeFolder/MyFile.txt`.

Note that [the Linux filesystem is different from the Windows filesystem](https://www.howtogeek.com/137096/6-ways-the-linux-file-system-is-different-from-the-windows-file-system/), and does not have a concept of drive letters (eg. `C:\`, `D:\`, etc). As a result, this method will only work properly for relative file paths.
