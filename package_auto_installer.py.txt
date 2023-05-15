import subprocess
import sys
import importlib

def install(package):
    subprocess.check_call([sys.executable, "-m", "pip", "install", package])

def main(script):
    with open(script, 'r') as f:
        lines = f.readlines()

    modules = []
    for line in lines:
        if "import " in line:
            modules.append(line.split()[1].split('.')[0])
        elif "from " in line and " import " in line:
            modules.append(line.split()[1].split('.')[0])

    for module in modules:
        try:
            importlib.import_module(module)
            print(f"{module} is already installed.")
        except ImportError:
            print(f"{module} is not installed. Installing now...")
            install(module)

    print("Running script...")
    subprocess.run([sys.executable, script])

if __name__ == "__main__":
    main(sys.argv[1])
