# Dynamic-Application-Launcher
i dont have JSON soo u might have to find urself sadly..


import subprocess
import platform
import os

def launch_app(app_path):
    """
    Launches an application based on the operating system.

    Args:
        app_path (str): The path to the application executable.
    """

    system = platform.system()

    try:
        if system == "Windows":
            subprocess.Popen([app_path], creationflags=subprocess.CREATE_NEW_CONSOLE) # Prevents console blocking in some cases
        elif system == "Darwin":  # macOS
            subprocess.Popen(["open", app_path])
        elif system == "Linux":
            # Attempt to launch with xdg-open, which is a common way to open files/apps
            subprocess.Popen(["xdg-open", app_path])
        else:
            print(f"Unsupported operating system: {system}")
            return # exit the function

        print(f"Successfully launched: {app_path}")

    except FileNotFoundError:
        print(f"Error: Application not found at {app_path}")
    except Exception as e:
        print(f"An error occurred: {e}")

# Example usage:
# Replace with the actual path to your application.

#Windows example
#app_path = r"C:\Path\To\Your\Application.exe"

#macOS example
#app_path = "/Applications/YourApp.app"

#linux example.
#app_path = "/path/to/your/application" #Or if it is a desktop file. "/path/to/YourApp.desktop"

#Example with a relative path (if the app is in the same directory as the python script)
#app_path = "./YourApp"

def launch_app_with_check(app_name):
  """
  Launches an application, attempting to find it in common locations.
  Args:
    app_name: the base name of the app.
  """
  system = platform.system()
  if system == "Windows":
      possible_paths = [
          os.path.join(os.getcwd(), app_name + ".exe"), # current directory
          os.path.join(os.path.expanduser("~"), "AppData", "Local", app_name, app_name + ".exe"), # Common install location
          os.path.join("C:\\Program Files\\", app_name, app_name + ".exe"),
          os.path.join("C:\\Program Files (x86)\\", app_name, app_name + ".exe")
      ]
  elif system == "Darwin": # macOS
      possible_paths = [
          os.path.join("/Applications/", app_name + ".app"),
          os.path.join(os.path.expanduser("~"), "Applications", app_name + ".app"),
          os.path.join(os.getcwd(), app_name + ".app"),
      ]
  elif system == "Linux":
      possible_paths = [
          os.path.join(os.getcwd(), app_name),
          os.path.join(os.path.expanduser("~"), ".local/share/applications/", app_name + ".desktop"), #desktop file
          "/usr/bin/" + app_name, #common bin location
          "/usr/local/bin/" + app_name
      ]
  else:
      print(f"Unsupported OS: {system}")
      return

  for path in possible_paths:
      if os.path.exists(path):
          launch_app(path)
          return
  print(f"Could not find application: {app_name}")

#Example usage with the app name.
#launch_app_with_check("MyApp")

#Choose one of the examples above and replace the placeholder app paths.
#If the application is not launched, double check the app path.

