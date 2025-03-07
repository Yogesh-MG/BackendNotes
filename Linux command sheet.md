
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Linux Command Cheat Sheet</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #1e1e2e;
            color: #cdd6f4;
            padding: 20px;
        }
        h1, h2 {
            color: #89b4fa;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
            background-color: #313244;
        }
        th, td {
            border: 1px solid #45475a;
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #45475a;
            color: #fab387;
        }
        tr:hover {
            background-color: #585b70;
        }
        code {
            background-color: #585b70;
            padding: 3px 6px;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    
    <h2>1. File & Directory Commands</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>pwd</code></td><td>Show current directory</td></tr>
        <tr><td><code>ls</code></td><td>List files and directories</td></tr>
        <tr><td><code>cd dir</code></td><td>Change directory</td></tr>
        <tr><td><code>mkdir dir</code></td><td>Create a directory</td></tr>
        <tr><td><code>rm file</code></td><td>Delete a file</td></tr>
    </table>
    
    <h2>2. File Permissions & Ownership</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>chmod 777 file</code></td><td>Change file permissions</td></tr>
        <tr><td><code>chown user file</code></td><td>Change file owner</td></tr>
    </table>
    
    <h2>3. Process Management</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>ps</code></td><td>Show running processes</td></tr>
        <tr><td><code>kill PID</code></td><td>Kill process by ID</td></tr>
        <tr><td><code>top</code></td><td>Show system tasks</td></tr>
    </table>
    
    <h2>4. Disk & Storage Commands</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>df -h</code></td><td>Show disk usage</td></tr>
        <tr><td><code>du -sh dir</code></td><td>Show size of a directory</td></tr>
    </table>
    
    <h2>5. Networking</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>ip a</code></td><td>Show IP addresses</td></tr>
        <tr><td><code>ping google.com</code></td><td>Check network connection</td></tr>
    </table>
    
    <h2>6. User Management</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>whoami</code></td><td>Show current user</td></tr>
        <tr><td><code>passwd user</code></td><td>Change user password</td></tr>
    </table>
    
    <h2>7. Package Management (Debian/Ubuntu)</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>apt update</code></td><td>Update package lists</td></tr>
        <tr><td><code>apt install pkg</code></td><td>Install a package</td></tr>
    </table>
    
    <h2>8. System Information</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>uname -a</code></td><td>Show system info</td></tr>
        <tr><td><code>free -h</code></td><td>Show memory usage</td></tr>
    </table>
    
    <h2>9. Compression & Archiving</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>tar -cvf file.tar dir/</code></td><td>Create tar archive</td></tr>
        <tr><td><code>zip file.zip file</code></td><td>Create ZIP file</td></tr>
    </table>
    
    <h2>10. SSH & Remote Access</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>ssh user@host</code></td><td>Connect to remote server</td></tr>
        <tr><td><code>scp file user@host:/dir</code></td><td>Secure copy file to remote</td></tr>
    </table>
    
    <h2>11. System Shutdown & Reboot</h2>
    <table>
        <tr><th>Command</th><th>Description</th></tr>
        <tr><td><code>shutdown now</code></td><td>Shutdown immediately</td></tr>
        <tr><td><code>reboot</code></td><td>Reboot system</td></tr>
    </table>
</body>
</html>
