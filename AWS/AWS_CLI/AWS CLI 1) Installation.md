# AWS CLI INSTALLATION

## DOCS

[For Windows](https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html)

[For Linux/Mac](https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-linux.html)

[For Amazon Linux](https://docs.aws.amazon.com/cli/latest/userguide/install-linux-al2017.html)

## WINDOWS

1. Download install file, make an installation

2. Verify success of installation by checking version installed (from command line):

```
aws --version

  aws-cli/1.15.81 Python/2.7.9 Windows/7 botocore/1.10.80
```

3. Verify user PATH var if utility is not found



## MAC

Upgrade pip for python3
```
pip3 install --upgrade pip
```

Install awscli
```
pip3 install awscli --upgrade --user
```


Find where awscli was intalled 

(pip installs executables to the same folder that contains the Python executable, so it's a python3 dir)
```
ls -la ~/Library/Python/3.7/bin
```

Add this dir to the path
```
echo 'export PATH=~/Library/Python/3.7/bin:$PATH' > ~/.bash_profile
```

In the new terminal session


Check that awscli is available
```
aws --version
```

## AMAZON LINUX

Use the curl command to download the installation script.
```
curl -O https://bootstrap.pypa.io/get-pip.py
```

Run the script with Python to download and install the latest version of pip and other required support packages.
```
python get-pip.py --user
```

Add pip3 to PATH
```
echo "export PATH=~/.local/bin:$PATH" >> ~/.bash_profile
source ~/.bash_profile
```

Use pip3 install to install the latest version of the AWS CLI. We recommend that if you have Python version 3+ installed that you use pip3.
```
pip2.7 install awscli --upgrade --user
```

Check that awscli is available
```
aws --version
```


















