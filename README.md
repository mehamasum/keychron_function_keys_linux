# Enable Function Keys On The Keychron K2 and K3 Mechanical Keyboard Under Linux


Below, you'll find the steps required to create a systemd command that will run at boot to disable the media keys and restore f1-f12 functionality.

## Step 1

Open a terminal window and create a new file in the folder \
_/etc/systemd/system/_.\
File name should be **keychron.service**

```bash
sudoedit /etc/systemd/system/keychron.service
```

### Note:

You can change the default editor to your preffered choice by typing `export
EDITOR=nano` before running `sudoedit`

## Step 2

Paste the following code into the window:

```
[Unit]
Description=The command to make the Keychron K2-k4 work with Function keys

[Service]
Type=oneshot
RemainAfterExit=true
ExecStart=/bin/bash -c "sudo echo 1 | sudo tee /sys/module/hid_apple/parameters/fnmode"
ExecStop=/bin/bash -c "sudo echo 0 | sudo tee /sys/module/hid_apple/parameters/fnmode"

[Install]
WantedBy=multi-user.target
```

To exit nvim press `escape` then `:` then `w` `q` and enter.

## Step 3

In the terminal, in order to enable the service type the following:

```bash
sudo systemctl enable keychron
```

## Step 4

To see the changes right away run last command:

```bash
sudo systemctl start keychron
```

Alternatively just run:

```bash
sudo systemctl enable --now keycron
```

## Closing Remarks

If you want to simply drag/drop the file that you create manually in the steps
provided, I have it under the scripts folder in this repo. Download it and drop
it in `/etc/systemd/system/`, doing Step 3 at the end.

### Toubleshooting

If this doesn't work swapping `ExecStart` and `ExecStop` might help.

```
[Unit]
Description=The command to make the Keychron K2-k4 work with Function keys

[Service]
Type=oneshot
RemainAfterExit=true
ExecStart=/bin/bash -c "sudo echo 0 | sudo tee /sys/module/hid_apple/parameters/fnmode"
ExecStop=/bin/bash -c "sudo echo 1 | sudo tee /sys/module/hid_apple/parameters/fnmode"

[Install]
WantedBy=multi-user.target
```
