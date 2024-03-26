## Requirements
The runtime requires Python 3.10 or later and the following packages:
`setup-tools` in version 61.0.0 or later.

### Check the Python version

Check if you have the correct version of Python installed. To do this, open a terminal and run the following command:

on Windows:
```bash
py --version
```

on Linux:
```bash
python3 --version
```
### Check the setup-tools package

Check if you have the `setup-tools` package installed. To do this, open a terminal and run the following command:

on Windows:
```bash
py -m ensurepip --upgrade
pip show setuptools
```

on Linux:
```bash
python3 -m ensurepip --upgrade
pip show setuptools
```

If the package is not installed, you can install it with the following command:

```bash
pip install setuptools
```

If the package is installed, but not up to date, you can update it with the following command:

```bash
pip install --upgrade setuptools
```


## Install the runtime

To install the runtime, you can use the following command:

```bash
pip install lemniscat-runtime
```

## Debug with VS Code

To debug your manifest with Visual Studio Code, you need to : 

1. At the root of your project create a folder `.vscode` 
2. Add a `launch.json` file and write this content :
    ```json
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "PowerShell: lem STG",
                "type": "PowerShell",
                "request": "launch",
                "script": "${workspaceRoot}/.vscode/run.ps1",
                "cwd": "${workspaceRoot}",
                "args": [
                    "-manifest",
                    "<path to manifest.yml>",
                    "-steps",
                    "'[\"all:all\"]'",
                    "-configFiles",
                    "'[\"<path to config1.json>\", \"<path to config2.json>\", <...>]'",
                    "-verbose",
                    "DEBUG",
                    "-outputPath",
                    "<path to output.json>",
                    "-extraParams",
                    "'{ \"appName\": \"sampl4\", \"scopeType\": \"developer\", \"developerEmail\": \"p.morisseau@groupeonepoint.com\", \"teamName\": \"\", \"instanceCode\": \"1\", <...> }'",
                ]
            }
        ]
    }
    ```
3. In `launch.json` replace content `<>` with your specific paths or values.
4. Add a `run.ps1` file and write this content :
    ```powershell
    param(
        [string]$manifest,
        [string]$steps,
        [string]$configFiles,
        [string]$verbose = "DEBUG",
        [string]$outputPath = "outputContext.json",
        $extraParams
    )
    
    $env:ARM_SUBSCRIPTION_ID= "<subscriptionid>"
    $env:ARM_TENANT_ID= "<tenantid>"
    $env:ARM_CLIENT_ID= "<clientid>"
    $env:ARM_CLIENT_SECRET= "<clientsecret>"
    
    python -m pip install lemniscat.runtime>=0.5.2
    
    lem -m $manifest -s $steps -c $configFiles -v $verbose -o $outputPath -x $extraParams
    ```

### Setup virtual environment

I advise creating a virtual environment if you don't want to have conflicts between your python packages.
For this I suggest you use Venv.

Before creating your virtual environment, in the `.vscode` folder create the `requirements.txt` file and add the following content:
```txt
setuptools>=61.0
```
 
Then, in Vscode, do **Ctrl+Shift+P**
then search for `Python: Create Environment...`
At creation time, it will ask you to define the requirements file.
 
Afterwards, you just have to do an F5 and off you go!