# 🖥️ System Monitoring

top                # Real-time view of processes, CPU, and memory usage
htop               # Enhanced 'top' with color, mouse support, and better readability

# 🔍 Process Management

ps                 # Snapshot of current processes
ps aux             # Detailed list of all processes
pgrep <name>       # Find process IDs by name
pstree -p          # Display processes in a tree format with PIDs

# 🌐 Network Diagnostics

netstat -tuln      # List all active listening ports
tcpdump -i enX0    # Capture and analyze network packets on interface enX0
ping google.com    # Check connectivity to Google
traceroute google.com  # Trace the path packets take to Google

# 💾 Disk & Memory Usage

df -h              # Show disk space usage in human-readable format
du -sh <folder>    # Display size of a specific folder
free -h            # Provide memory usage statistics

# 📄 Log Management

journalctl -u <service>  # View logs for a specific systemd service
journalctl -b            # View logs since the last boot
sudo cat /var/log/auth.log       # View authentication logs
sudo tail -n 10 /var/log/auth.log  # View last 10 lines of the auth log

# 🔧 Miscellaneous Tools

lsof -i :<port>    # Identify processes using a specific port
history            # Display command history
Ctrl + R           # Search command history
export PS1="Prompt"  # Change the terminal prompt
export PS1="$pwd"    # sets teh curretn terminal prompt to pwd

