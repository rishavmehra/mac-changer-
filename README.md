 import subprocess
import optparse
import re

def get_arguments():
    rishu = optparse.OptionParser()
    rishu.add_option("-i", "--interface", dest="interface", help="Interface to its mac address")
    rishu.add_option("-m", "--mac", dest="new_mac", help="new mac address")
    (options, arguments) =  rishu.parse_args()
    if not options.interface:
        parser.error("[-] please the enter the interface , or use --help for info ")
    elif not options.new_mac:
        parser.error("[-] please the enter the NEW MAC , or use --help for info")
    return options

def rishu_mac_change(interface,new_mac):
    print("[+] change mac address for " + interface + " to " + new_mac)
    subprocess.call(["ifconfig", interface, "down"])
    subprocess.call(["ifconfig", interface, "hw", "ether", new_mac])
    subprocess.call(["ifconfig", interface, "up"])

def get_current_mac(interface):
    ifconfig_result = subprocess.check_output(["ifconfig",  interface])
    mac_search_ifconfig = re.search(r"\w\w:\w\w:\w\w:\w\w:\w\w:\w\w", ifconfig_result)
    if mac_search_ifconfig:
         return mac_search_ifconfig.group(0)
    else:
        print("[-] mac address could not find . ")

options = get_arguments()
current_mac = get_current_mac(options.interface)
print("Current mac " + str(current_mac))
rishu_mac_change(options.interface,options.new_mac)
current_mac = get_current_mac(options.interface)
if current_mac == options.new_mac:
    print(" [+] MAC address successfully  changed " + current_mac)
else:
    print (" [-] MAC address unsccessfully to change .")


