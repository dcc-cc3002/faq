# Connecting to GitHub with SSH

You can connect to GitHub using the Secure Shell Protocol (SSH), which provides a secure channel 
over an unsecured network.

## About SSH

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh

## Generating a new SSH key and adding it to the ssh-agent

1. Open *PowerShell* (with *Windows Terminal*)
2. Paste the text below, substituting in your GitHub email address.
```powershell
ssh-keygen.exe -t ed25519 -C "your_email@example.com"
```
This creates a new SSH key, using the provided email as a label.
```
> Generating public/private algorithm key pair.
```
> Si el comando anterior falla, tienen que instalar OpenSSH, pueden encontrar instrucciones para
> instalar aquí: https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui
3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.
```
> Enter file in which to save the key (C:\Users\User/.ssh/id_ed25519):
```
Anoten el path del archivo (en este caso `C:\Users\User/.ssh/id_ed25519`)
4. At the prompt, type a secure passphrase.
```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

## Adding your SSH key to the ssh-agent

Add your SSH private key to the ssh-agent.
Reemplacen la ruta con el path que guardaron antes
```powershell
ssh-add C:\Users\User\.ssh\id_ed25519
```

## Adding a new SSH key to your GitHub account

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

## Testing your SSH connection

https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection