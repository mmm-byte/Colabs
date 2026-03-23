# SuperPOD HPC Guide

This guide provides instructions for using the SuperPOD HPC cluster. It covers how to access the cluster, run jupyter notebooks, submit jobs, and manage your resources effectively.

For an extensive tutorial on using SuperPOD, please refer to the [SuperPOD HPC Guide](https://southernmethodistuniversity.github.io/SMU_SuperPOD_101/).

## Accessing SuperPOD
To access the SuperPOD cluster, you will need to use SSH (Secure Shell) from your terminal. Accessing via SSH will require you to be in the SMU network or connected through a VPN. Meaning if you are on-campus, you can access it directly on PerunaNet. If you are off-campus, you will need to connect to the SMU VPN before you can access SuperPOD.

### Connecting to the SMU VPN
If you are off-campus, you will need to connect to the SMU VPN to access SuperPOD. You can download the VPN client from the SMU IT website. Once you have the VPN client installed, follow the instructions to connect to the SMU network.
1. Go to the [SMU VPN page](https://www.smu.edu/oit/services/vpn).
2. Download and install the Cisco Secure Client. Follow the installation instructions provided on the website.
3. Once installed, open the Cisco Secure Client and type in `vpn.smu.edu`. Hit connect and log in with your SMU credentials.

Once you are connected to PerunaNet or connected to the SMU VPN, you can use the following command:

```bash
ssh -C -D 8080 <your_smu_username>@superpod.smu.edu
```

Or, connect to your IDE (e.g., Visual Studio Code) using the Remote SSH extension with the same credentials.

1. Go to the Remote Explorer in Visual Studio Code.
2. Click on "SSH Targets" and then "Add New SSH Host".
3. Enter the SSH command: `ssh -C -D 8080 <your_smu_username>@superpod.smu.edu`.
4. Follow the prompts to connect to SuperPOD.

## Git Clone the Repository in your Home Directory
Once you have access to SuperPOD, you can clone the repository containing the lab materials.
```bash
cd ~
git clone https://github.com/JerMa88/CS7325.git
```
This will create a directory called `CS7325` in your home directory with all the necessary files for the lab. 

## Running Jupyter Notebooks on SuperPOD
To run Jupyter notebooks on SuperPOD, you will need to set up a Jupyter server. Follow these steps to get it running:

### Set Up Browser Proxy
To access the Jupyter server running on SuperPOD, you need to set up a proxy in your browser. This will allow you to access the Jupyter server through the SSH tunnel you created with the `ssh -C -D 8080` command.

Setup browser to enable proxy viewing

Using Firefox as browser:
open Firefox, open up the settings tab. Scroll to bottom and select Settings from Network Settings.

1. Select Manual Proxy Configuration
2. In the SOCKS Host, enter localhost, Port 8000
3. Check SOCKS v5.
4. Check Proxy DNS when using SOCKS v5.
5. Check Enable DNS over HTTPS.
6. Make sure everything else is unchecked, then click OK.

Your screenshot should look like below:

![FireFox Proxy Settings](https://user-images.githubusercontent.com/43855029/189716896-4415fb80-9b1f-4287-9ecf-6adc2b1357ef.png)

Test Proxy using conda environment:

Go back to your terminal or your IDE with the SSH tunnel established and login into SuperPOD using regular SSH Request a compute node.

```bash
srun -A eclarson_cs5325_1262_001c_0001 -N1 -G1 -c8 --mem=64G --time=12:00:00 --pty $SHELL
```

Cuda should be automatically loaded upon GPU resource allocation. Just activate any of your virtual or conda environment, for example venv in the repo directory

```bash
source ~/CS7325/.venv/bin/activate
```
(or a conda environment if you have one set up. [Tutorial on setting up conda environments](https://southernmethodistuniversity.github.io/SMU_SuperPOD_101/02-Working%20with%20Conda/index.html))

Then run the following command to test if the proxy is working:   
Make sure to install jupyter

```bash
pip install jupyterlab
```

Next insert the following command:
```bash
jupyter notebook --ip=0.0.0.0 --no-browser
```
or
```bash
$ jupyter lab --ip=0.0.0.0 --no-browser   
```

The following screen appears

![Jupyter Notebook Output](https://user-images.githubusercontent.com/43855029/211889136-0eb5ef90-b306-454b-8fd9-8fab290b79b4.png)

Copy the highlighted URLs to Firefox, you will see Jupyter Notebook port forward to this:

![Firefox Jupyter Notebook](https://user-images.githubusercontent.com/43855029/211889462-ea4ebe65-9f2f-4bc4-9980-493ffe74bed7.png)

Select TensorflowGPU29 kernel notebook and Check GPU device:

![Check GPU Device](https://user-images.githubusercontent.com/43855029/211889805-da9d0740-3383-4b74-a347-b16525708ba3.png)

### Debug
If you cannot find the jupyter kernel or the notebook cannot connect to the kernel, you can try the following steps:
1. Make sure you have activated the correct virtual or conda environment that has the jupyter kernel installed. You can check the available kernels by running `jupyter kernelspec list` in your terminal.
2. If the kernel is not listed, you may need to install it in your environment. For example, if you are using a conda environment, you can install the kernel by activating the environment and running `pip install ipykernel` and then add it to Jupyter with `python -m ipykernel install --user --name=<your_env_name>`. Make sure to replace `<your_env_name>` with the name of your environment. A more detailed tutorial on setting up conda environments and Jupyter kernels can be found in the [tutorial on setting up conda environments](https://southernmethodistuniversity.github.io/SMU_SuperPOD_101/02-Working%20with%20Conda/index.html).
