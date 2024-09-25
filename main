#!/bin/bash

# Function to display the menu
show_menu() {
    echo "========================="
    echo "       MAIN MENU"
    echo "========================="
    echo "1. Install Netplan"
    echo "2. Local Iran"
    echo "3. Local Kharej"
    echo "4. Ping Tunnel"
    echo "5. Delete Tunnels"
    echo "6. Bypass Iran Server Errors"
    echo "7. Exit"
}

# Function to install Netplan
install_netplan() {
    echo "Installing Netplan..."
    sudo apt-get install iproute2
    sudo apt install netplan.io -y
    echo "Netplan installed."
}

# Function to ask for IP addresses, create netplan file, and apply changes for Local Iran
enter_ip_addresses_and_apply_netplan() {
    read -p "Enter IP for Iran: " ip_iran
    read -p "Enter IP for Kharej: " ip_kharej

    echo "Creating Netplan file with the following details:"
    echo "IP-Iran: $ip_iran"
    echo "IP-Kharej: $ip_kharej"

    # Create the Netplan configuration file
    sudo bash -c "cat > /etc/netplan/tunnel.yaml" <<EOL
network:
  version: 2
  tunnels:
    tunnel01:
      mode: sit
      local: $ip_iran
      remote: $ip_kharej
      addresses:
        - 2001:aa1:111::1/64
EOL

    echo "Netplan configuration file /etc/netplan/tunnel.yaml created."

    # Apply the Netplan changes
    echo "Applying Netplan changes..."
    sudo netplan apply
    echo "Netplan changes applied successfully."
}

# Function for Local Kharej, asking for variables in reverse order
local_kharej() {
    read -p "Enter IP for Kharej: " ip_kharej
    read -p "Enter IP for Iran: " ip_iran

    echo "Creating Netplan file with the following details:"
    echo "IP-Kharej: $ip_kharej"
    echo "IP-Iran: $ip_iran"

    # Create the Netplan configuration file
    sudo bash -c "cat > /etc/netplan/tunnel.yaml" <<EOL
network:
  version: 2
  tunnels:
    tunnel01:
      mode: sit
      local: $ip_kharej
      remote: $ip_iran
      addresses:
        - 2001:aa1:111::2/64
EOL

    echo "Netplan configuration file /etc/netplan/tunnel.yaml created."

    # Apply the Netplan changes
    echo "Applying Netplan changes..."
    sudo netplan apply
    echo "Netplan changes applied successfully."
}

# Function for Ping Tunnel sub-options
ping_tunnel() {
    echo "========================="
    echo "     PING TUNNEL"
    echo "========================="
    echo "1. Ping Kharej"
    echo "2. Ping Iran"
    echo "3. Back to Main Menu"

    read -p "Choose an option [1-3]: " sub_option
    case $sub_option in
        1) 
            echo "Pinging Kharej at 2001:aa1:111::2/64..."
            ping -c 4 2001:aa1:111::2
            ;;
        2) 
            echo "Pinging Iran at 2001:aa1:111::1/64..."
            ping -c 4 2001:aa1:111::1
            ;;
        3) 
            return ;;
        *) 
            echo "Invalid option, returning to main menu." ;;
    esac
}

# Function to delete the tunnels
delete_tunnels() {
    echo "Deleting tunnel configuration..."
    if [ -f /etc/netplan/tunnel.yaml ]; then
        sudo rm /etc/netplan/tunnel.yaml
        echo "Tunnel configuration file /etc/netplan/tunnel.yaml deleted."
        echo "Applying Netplan changes..."
        sudo netplan apply
        echo "Netplan changes applied successfully."
    else
        echo "No tunnel configuration file found."
    fi
}

# Function to bypass Iran server errors
bypass_iran_errors() {
    echo "Bypassing Iran server errors..."
    
    # Commands to unmask and restart systemd-networkd, then apply Netplan
    sudo systemctl unmask systemd-networkd
    sudo systemctl restart systemd-networkd
    sudo netplan apply

    echo "Bypass completed. Network configuration has been reapplied."
}

# Main script loop
while true
do
    show_menu
    read -p "Choose an option [1-7]: " option
    case $option in
        1) install_netplan ;;
        2) enter_ip_addresses_and_apply_netplan ;;
        3) local_kharej ;;
        4) ping_tunnel ;;
        5) delete_tunnels ;;
        6) bypass_iran_errors ;;
        7) echo "Exiting..."; exit 0 ;;
        *) echo "Invalid option, please try again." ;;
    esac
    echo ""
done
