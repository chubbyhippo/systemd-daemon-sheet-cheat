# systemd-daemon-sheet-cheat
# Guide: Creating, Starting, and Stopping a systemd Service (Execute `.sh` Script)

This guide explains how to create a `systemd` service, specifically designed to execute a `.sh` script, and manage the service.

---

## 1. Create a Custom systemd Service File

1. **Location of the service file**:
   Service files for `systemd` are stored in `/lib/systemd/system/` or `/etc/systemd/system/`.
   Use `/etc/systemd/system/` for custom services.

2. **Create the service file**:
   ```bash
   sudo nano /etc/systemd/system/<service-name>.service
   ```

3. **Template for the service file (to execute an `.sh` file)**:
   Replace `<service-name>` and paths with your custom values.

   ```ini
   [Unit]
   Description=Description of your service
   After=network.target

   [Service]
   Type=simple
   ExecStart=/bin/bash /path/to/your-script.sh
   Restart=on-failure
   User=<user>
   WorkingDirectory=<path/to/working/directory>
   
   [Install]
   WantedBy=multi-user.target
   ```

   - **`ExecStart`**: Provide the absolute path to `bash` and your `.sh` file.
   - **`User`**: The user account under which the script should run. Use `root` or a specific user account (e.g., `User=example`).
   - **`WorkingDirectory`**: Specify the directory where the `.sh` script will execute (optional but useful if the script depends on relative file paths).

4. **Save and exit**:
   Save changes and close the editor (`Ctrl+O`, `Enter`, `Ctrl+X` in Nano).

---

## 2. Reload systemd to Recognize the Service

Run the following command to reload `systemd` and recognize the service file:

```bash
sudo systemctl daemon-reload
```

---

## 3. Start the Service

To start the service, use:

```bash
sudo systemctl start <service-name>
```

Check the service status:

```bash
sudo systemctl status <service-name>
```

---

## 4. Enable the Service to Start on Boot

To enable the service:

```bash
sudo systemctl enable <service-name>
```

---

## 5. Stop the Service

To stop the service:

```bash
sudo systemctl stop <service-name>
```

---

## 6. Disable the Service

To disable the service from starting on boot:

```bash
sudo systemctl disable <service-name>
```

---

## 7. Restart the Service

To restart the service:

```bash
sudo systemctl restart <service-name>
```

---

## 8. Logs and Troubleshooting

View logs for your service using `journalctl`:

```bash
sudo journalctl -u <service-name>
```

Use this to debug any issues or logs generated by your `.sh` script.

---

## Example

Here’s an example of a service definition to execute a script `/home/example/myscript.sh` as the user `example`.

1. Service file:

   ```ini
   [Unit]
   Description=My Shell Script Service
   After=network.target

   [Service]
   Type=simple
   ExecStart=/bin/bash /home/example/myscript.sh
   Restart=on-failure
   User=example
   WorkingDirectory=/home/example
   
   [Install]
   WantedBy=multi-user.target
   ```

2. Save it as `/etc/systemd/system/myscript.service`.

3. Reload and manage the service:
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start myscript.service
   sudo systemctl enable myscript.service
   ```

4. Check the service status:
   ```bash
   sudo systemctl status myscript.service
   ```

5. Check logs if needed:
   ```bash
   sudo journalctl -u myscript.service
   ```

---

## Notes

- Ensure the `.sh` file is executable:
  ```bash
  chmod +x /path/to/your-script.sh
  ```
- Debug the `.sh` script independently to ensure it works as expected before linking it to `systemd`.

---

With this guide, you’ll be able to execute `.sh` scripts using custom `systemd` services and manage them efficiently!
