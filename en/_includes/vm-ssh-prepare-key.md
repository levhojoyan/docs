To connect to a [VM](../compute/concepts/vm.md) over SSH, you need a key pair: the public key is placed on the VM, while the private one is stored on the user's device. This method is more secure than connecting with a username and password.

{% include [vm-connect-ssh-linux-note](vm-connect-ssh-linux-note.md) %}

To create a key pair:

{% list tabs %}

- Linux/MacOS

   1. Open the terminal.
   1. Use the `ssh-keygen` command to create a new key:

      ```bash
      ssh-keygen -t ed25519
      ```

      After you run the command, you will be asked to specify the names of files where the keys will be saved and enter the password for the private key. Press **Enter** to use the default name (`id_ed25519`). The key pair will be created in the `~/.ssh` directory.

      The public key will be saved in the `<key_name>.pub` file.

- Windows 10

   If you do not have [OpenSSH](https://en.wikipedia.org/wiki/OpenSSH) installed yet, install it by following this [guide](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui).

   1. Run `cmd.exe` or `powershell.exe` (make sure to update PowerShell first).
   1. Use the `ssh-keygen` command to create a new key. Run this command:

      ```shell
      ssh-keygen -t ed25519
      ```

      After you run the command, you will be asked to specify the names of files where the keys will be saved and enter the password for the private key. Press **Enter** to use the default name (`id_ed25519`).

      The key pair will be created in `C:\Users\<username>\.ssh\` or `C:\Users\<username>\` depending on the command line interface.

      The public key will be saved in the `<key_name>.pub` file.

- Windows 7/8

   To create keys for Windows, use the PuTTY application.
   1. [Download](https://www.putty.org) and install PuTTY.
   1. Make sure that the directory where you installed PuTTY is included in `PATH`:
      1. Right-click **My computer**. Click **Properties**.
      1. In the window that opens, select **Additional system parameters**, then **Environment variables** (located in the lower part of the window).
      1. Under **System variables**, find `PATH` and click **Edit**.
      1. In the **Variable value** field, append the path to the directory where you installed PuTTY.
   1. Launch the PuTTYgen app.
   1. Select **EdDSA** as the pair type to generate. Click **Generate** and move the cursor in the field above it until key creation is complete.

      ![ssh_generate_key](../_assets/compute/ssh-putty/ssh_generate_key.png)

   1. In **Key passphrase**, enter a strong password. Enter it again in the field below.
   1. Click **Save private** key and save the private key. Never share it with anyone and do not tell anyone the passphrase for it.
   1. Save the key to a text file. To do this, copy the **single-line** public key from the text field to a text file named `id_ed25519.pub`.

{% endlist %}