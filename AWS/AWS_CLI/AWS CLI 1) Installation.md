# AWS CLI INSTALLATION

## DOCS

[For Windows](https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-windows.html)

[For Linux/Mac](https://docs.aws.amazon.com/cli/latest/userguide/awscli-install-linux.html)


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




















