
# Security - Firewall
## Introduction

ใน **Linux kernel** มีส่วนประกอบที่เรียกว่า **Netfilter** ซึ่งใช้สำหรับการจัดการหรือตัดสินใจเกี่ยวกับการส่งข้อมูลเครือข่ายเข้าหรือผ่านเซิร์ฟเวอร์ของคุณ โซลูชันไฟร์วอล Linux ทุกตัวใช้ระบบนี้สำหรับการกรองแพ็กเก็ตหากไม่มีอินเตอร์เฟซของผู้ใช้ในพื้นที่การทำงาน ระบบการกรองแพ็กเก็ตของ kernel จะไม่เป็นประโยชน์ต่อผู้ดูแลระบบ และนั่นเป็นเหตุผลที่มี iptables มาช่วยเมื่อแพ็กเก็ตมาถึงที่เซิร์ฟเวอร์ของคุณ มันจะถูกส่งไปยังส่วนประกอบ Netfilter เพื่อตอบรับ การจัดการ หรือปฏิเสธขึ้นอยู่กับกฎที่ได้รับจากพื้นที่ทำงานผู้ใช้ผ่าน iptables ดังนั้น iptables เพียงพอสำหรับการจัดการไฟร์วอลของคุณหากคุณคุ้นเคยกับมัน แต่มีอินเตอร์เฟซหลายรูปแบบที่มีให้ใช้งานเพื่อความง่ายขึ้น

## ufw - Uncomplicated Firewall

เครื่องมือการกำหนดค่าไฟร์วอลที่เริ่มต้นสำหรับ Ubuntu คือ ufw พัฒนาขึ้นเพื่อสะดวกในการกำหนดค่าไฟร์วอล iptables ufw ให้วิธีการใช้งานที่ใช้ง่ายในการสร้างไฟร์วอลเบสเริ่มต้น IPv4 หรือ IPv6โดยปกติ ufw จะถูกปิดใช้งานตั้งแต่เริ่มต้น ตาม ufw man page: "ufw ไม่ได้มีวัตถุประสงค์ที่จะให้ฟังก์ชันไฟร์วอลที่สมบูรณ์ผ่านอินเตอร์เฟซคำสั่งของมัน แต่มีวัตถุประสงค์ในการให้วิธีง่ายๆในการเพิ่มหรือลบกฎที่เรียบง่าย ๆ ปัจจุบันใช้สำหรับไฟร์วอลเบสเริ่มต้น"
##

 **ด้านล่างนี้เป็นตัวอย่างของวิธีการใช้งาน ufw**
 

แนวทางการใช้งาน ufw (Uncomplicated Firewall) เพื่อกำหนดกฎการทำงานของไฟวอลล์ในระบบ Linux โดยใช้คำสั่งต่าง ๆ ตามด้านล่างนี้:

**- เปิดใช้งาน ufw:**

    sudo ufw enable
    

 **- เปิดพอร์ต (ตัวอย่างเปิดพอร์ต SSH):**

    sudo ufw allow 22

 **- การเพิ่มกฎโดยระบุเลขลำดับ:**

	sudo ufw insert 1 allow 80

 **- ปิดพอร์ตที่เปิดไว้:**

	sudo ufw deny 22

 **- การลบกฎโดยระบุกฎ:**

	sudo ufw delete deny 22

 **- อนุญาตการเข้าถึงจากโฮสต์หรือเครือข่ายที่ระบุ:**

	sudo ufw allow proto tcp from 192.168.0.2 to any port 22

> แทนที่ 192.168.0.2 ด้วย 192.168.0.0/24 เพื่ออนุญาตการเข้าถึง SSH จากทั้งเครือข่ายทั้งหมดใน subnet.

**- เพิ่มคำสั่ง --dry-run เพื่อดูกฎที่จะถูกสร้างโดย ufw แต่ไม่นำไปใช้:**

	sudo ufw --dry-run allow http
 **- ปิดใช้งาน ufw:**

	sudo ufw disable

 **- ตรวจสอบสถานะของไฟวอลล์:**

	sudo ufw status

 **- ตรวจสอบสถานะของไฟวอลล์แบบ verbose:**

	sudo ufw status verbose 

 **- ดูกฎในรูปแบบเลขลำดับ:**

	sudo ufw status numbered

> **Note**
> 
> หากพอร์ตที่คุณต้องการเปิดหรือปิดถูกกำหนดไว้ใน `/etc/services`, คุณสามารถใช้ชื่อพอร์ตแทนตัวเลขได้ ในตัวอย่างข้างต้นนี้ ให้แทน _22_ ด้วย _ssh_.

## 
## ufw Application Integration


แอปพลิเคชันที่เปิดพอร์ตสามารถรวมโปรไฟล์ ufw เข้าไปได้ โดยโปรไฟล์จะระบุพอร์ตที่ต้องการให้แอปพลิเคชันทำงานอย่างถูกต้อง โปรไฟล์ถูกเก็บไว้ที่ `/etc/ufw/applications.d` และสามารถแก้ไขได้หากพอร์ตเริ่มต้นได้รับการเปลี่ยนแปลง

**- เพื่อดูว่าแอปพลิเคชันไหนได้ติดตั้งโปรไฟล์แล้วให้ป้อนคำสั่งต่อไปนี้ในเทอร์มินัล:**

    sudo ufw app list 

**- เหมือนกับการอนุญาตให้ไฟวอลล์ใช้งานพอร์ต การใช้โปรไฟล์แอปพลิเคชันสามารถทำได้โดยป้อน:**

    sudo ufw allow Samba 

**- ยังมีไวยากรณ์เสริมให้ใช้งาน:**

    ufw allow from 192.168.0.0/24 to any app Samba 

> แทน _Samba_ และ _192.168.0.0/24_
> ด้วยโปรไฟล์แอปพลิเคชันที่คุณกำลังใช้และช่วง IP ของเครือข่ายของคุณ

> **Note**
> 
> ไม่จำเป็นต้องระบุ _โปรโตคอล_ สำหรับแอปพลิเคชัน เนื่องจากข้อมูลนั้นระบุในโปรไฟล์ อีกทั้ง _ชื่อแอป_ จะแทน _หมายเลขพอร์ต_

**- เพื่อดูรายละเอียดเกี่ยวกับพอร์ต โปรโตคอล และอื่น ๆ ที่ถูกกำหนดให้ป้อน:**

    sudo ufw app info Samba 

ไม่ใช่ทุกแอปพลิเคชันที่ต้องการเปิดพอร์ตเครือข่ายมาพร้อมกับโปรไฟล์ ufw แต่หากคุณได้ทำโปรไฟล์สำหรับแอปพลิเคชันแล้ว และต้องการให้ไฟล์ถูกรวมในแพ็กเกจ กรุณาแจ้งข้อผิดพลาดที่แพ็กเกจใน Launchpad

    ubuntu-bug nameofpackage

# iptables
Linux Command – iptables ใช้ในการจัดการกรอง  ip port ที่เข้ามาใช้งาน

## คำสั่ง

**1. แสดงข้อมูลการกรอง conncetion**

    $ iptables -L INPUT -n --line-numbers
**2. ลบ iptables rule**

    $ iptables -D INPUT <line-number>

**3. เพิ่ม iptables rule**

    $ iptables -I INPUT 2 -s 202.54.1.2 -j DROP

### โครงสร้างคำสั่ง

     iptables [-t table] {-A|-C|-D} chain rule-specification
     ip6tables [-t table] {-A|-C|-D} chain rule-specification
     iptables [-t table] -I chain [rulenum] rule-specification
     iptables [-t table] -R chain rulenum rule-specification
     iptables [-t table] -D chain rulenum
     iptables [-t table] -S [chain [rulenum]]
     iptables [-t table] {-F|-L|-Z} [chain [rulenum]] [options...]
     iptables [-t table] -N chain
     iptables [-t table] -X [chain]
     iptables [-t table] -P chain target
     iptables [-t table] -E old-chain-name new-chain-name
     rule-specification = [matches...] [target]
     match = -m matchname [per-match-options]
     target = -j targetname [per-target-options] 

### รายละเอียด
เป็นคำสั่งที่ใช้ในการจัดการกรอง ip port ที่เข้ามาใช้งาน หรือ การทำ **packet filtering (IP/Port)**



### Option
    COMMANDS
     These options specify the desired action to perform. Only one of them can be specified on the command line unless otherwise stated below. For long versions of the command and
     option names, you need to use only enough letters to ensure that iptables can differentiate it from all other options.
    
     -A, --append chain rule-specification
     Append one or more rules to the end of the selected chain. When the source and/or destination names resolve to more than one address, a rule will be added for each possible
     address combination.
    
     -C, --check chain rule-specification
     Check whether a rule matching the specification does exist in the selected chain. This command uses the same logic as -D to find a matching entry, but does not alter the
     existing iptables configuration and uses its exit code to indicate success or failure.
    
     -D, --delete chain rule-specification
     -D, --delete chain rulenum
     Delete one or more rules from the selected chain. There are two versions of this command: the rule can be specified as a number in the chain (starting at 1 for the first
     rule) or a rule to match.
    
     -I, --insert chain [rulenum] rule-specification
     Insert one or more rules in the selected chain as the given rule number. So, if the rule number is 1, the rule or rules are inserted at the head of the chain. This is also
     the default if no rule number is specified.
    
     -R, --replace chain rulenum rule-specification
     Replace a rule in the selected chain. If the source and/or destination names resolve to multiple addresses, the command will fail. Rules are numbered starting at 1.
    
     -L, --list [chain]
     List all rules in the selected chain. If no chain is selected, all chains are listed. Like every other iptables command, it applies to the specified table (filter is the
     default), so NAT rules get listed by
     iptables -t nat -n -L
     Please note that it is often used with the -n option, in order to avoid long reverse DNS lookups. It is legal to specify the -Z (zero) option as well, in which case the
     chain(s) will be atomically listed and zeroed. The exact output is affected by the other arguments given. The exact rules are suppressed until you use
     iptables -L -v
    
     -S, --list-rules [chain]
     Print all rules in the selected chain. If no chain is selected, all chains are printed like iptables-save. Like every other iptables command, it applies to the specified
     table (filter is the default).
    
     -F, --flush [chain]
     Flush the selected chain (all the chains in the table if none is given). This is equivalent to deleting all the rules one by one.
    
     -Z, --zero [chain [rulenum]]
     Zero the packet and byte counters in all chains, or only the given chain, or only the given rule in a chain. It is legal to specify the -L, --list (list) option as well, to
     see the counters immediately before they are cleared. (See above.)
    
     -N, --new-chain chain
     Create a new user-defined chain by the given name. There must be no target of that name already.
    
     -X, --delete-chain [chain]
     Delete the optional user-defined chain specified. There must be no references to the chain. If there are, you must delete or replace the referring rules before the chain
     can be deleted. The chain must be empty, i.e. not contain any rules. If no argument is given, it will attempt to delete every non-builtin chain in the table.
    
     -P, --policy chain target
     Set the policy for the built-in (non-user-defined) chain to the given target. The policy target must be either ACCEPT or DROP.
    
     -E, --rename-chain old-chain new-chain
     Rename the user specified chain to the user supplied name. This is cosmetic, and has no effect on the structure of the table.
    
     -h Help. Give a (currently very brief) description of the command syntax.
    
     PARAMETERS
     The following parameters make up a rule specification (as used in the add, delete, insert, replace and append commands).
    
     -4, --ipv4
     This option has no effect in iptables and iptables-restore. If a rule using the -4 option is inserted with (and only with) ip6tables-restore, it will be silently ignored.
     Any other uses will throw an error. This option allows IPv4 and IPv6 rules in a single rule file for use with both iptables-restore and ip6tables-restore.
    
     -6, --ipv6
     If a rule using the -6 option is inserted with (and only with) iptables-restore, it will be silently ignored. Any other uses will throw an error. This option allows IPv4 and
     IPv6 rules in a single rule file for use with both iptables-restore and ip6tables-restore. This option has no effect in ip6tables and ip6tables-restore.
    
     [!] -p, --protocol protocol
     The protocol of the rule or of the packet to check. The specified protocol can be one of tcp, udp, udplite, icmp, icmpv6,esp, ah, sctp, mh or the special keyword "all", or
     it can be a numeric value, representing one of these protocols or a different one. A protocol name from /etc/protocols is also allowed. A "!" argument before the protocol
     inverts the test. The number zero is equivalent to all. "all" will match with all protocols and is taken as default when this option is omitted. Note that, in ip6tables,
     IPv6 extension headers except esp are not allowed. esp and ipv6-nonext can be used with Kernel version 2.6.11 or later. The number zero is equivalent to all, which means
     that you cannot test the protocol field for the value 0 directly. To match on a HBH header, even if it were the last, you cannot use -p 0, but always need -m hbh.
    
     [!] -s, --source address[/mask][,...]
     Source specification. Address can be either a network name, a hostname, a network IP address (with /mask), or a plain IP address. Hostnames will be resolved once only,
     before the rule is submitted to the kernel. Please note that specifying any name to be resolved with a remote query such as DNS is a really bad idea. The mask can be
     either an ipv4 network mask (for iptables) or a plain number, specifying the number of 1's at the left side of the network mask. Thus, an iptables mask of 24 is equivalent
     to 255.255.255.0. A "!" argument before the address specification inverts the sense of the address. The flag --src is an alias for this option. Multiple addresses can be
     specified, but this will expand to multiple rules (when adding with -A), or will cause multiple rules to be deleted (with -D).
    
     [!] -d, --destination address[/mask][,...]
     Destination specification. See the description of the -s (source) flag for a detailed description of the syntax. The flag --dst is an alias for this option.
    
     -m, --match match
     Specifies a match to use, that is, an extension module that tests for a specific property. The set of matches make up the condition under which a target is invoked. Matches
     are evaluated first to last as specified on the command line and work in short-circuit fashion, i.e. if one extension yields false, evaluation will stop.
    
     -j, --jump target
     This specifies the target of the rule; i.e., what to do if the packet matches it. The target can be a user-defined chain (other than the one this rule is in), one of the
     special builtin targets which decide the fate of the packet immediately, or an extension (see EXTENSIONS below). If this option is omitted in a rule (and -g is not used),
     then matching the rule will have no effect on the packet's fate, but the counters on the rule will be incremented.
    
     -g, --goto chain
     This specifies that the processing should continue in a user specified chain. Unlike the --jump option return will not continue processing in this chain but instead in the
     chain that called us via --jump.
    
     [!] -i, --in-interface name
     Name of an interface via which a packet was received (only for packets entering the INPUT, FORWARD and PREROUTING chains). When the "!" argument is used before the inter‐
     face name, the sense is inverted. If the interface name ends in a "+", then any interface which begins with this name will match. If this option is omitted, any interface
     name will match.
    
     [!] -o, --out-interface name
     Name of an interface via which a packet is going to be sent (for packets entering the FORWARD, OUTPUT and POSTROUTING chains). When the "!" argument is used before the
     interface name, the sense is inverted. If the interface name ends in a "+", then any interface which begins with this name will match. If this option is omitted, any
     interface name will match.
    
     [!] -f, --fragment
     This means that the rule only refers to second and further IPv4 fragments of fragmented packets. Since there is no way to tell the source or destination ports of such a
     packet (or ICMP type), such a packet will not match any rules which specify them. When the "!" argument precedes the "-f" flag, the rule will only match head fragments, or
     unfragmented packets. This option is IPv4 specific, it is not available in ip6tables.
    
     -c, --set-counters packets bytes
     This enables the administrator to initialize the packet and byte counters of a rule (during INSERT, APPEND, REPLACE operations).
    
     OTHER OPTIONS
     The following additional options can be specified:
    
     -v, --verbose
     Verbose output. This option makes the list command show the interface name, the rule options (if any), and the TOS masks. The packet and byte counters are also listed,
     with the suffix 'K', 'M' or 'G' for 1000, 1,000,000 and 1,000,000,000 multipliers respectively (but see the -x flag to change this). For appending, insertion, deletion and
     replacement, this causes detailed information on the rule or rules to be printed. -v may be specified multiple times to possibly emit more detailed debug statements.
    
     -w, --wait [seconds]
     Wait for the xtables lock. To prevent multiple instances of the program from running concurrently, an attempt will be made to obtain an exclusive lock at launch. By
     default, the program will exit if the lock cannot be obtained. This option will make the program wait (indefinitely or for optional seconds) until the exclusive lock can be
     obtained.
    
     -n, --numeric
     Numeric output. IP addresses and port numbers will be printed in numeric format. By default, the program will try to display them as host names, network names, or services
     (whenever applicable).
    
     -x, --exact
     Expand numbers. Display the exact value of the packet and byte counters, instead of only the rounded number in K's (multiples of 1000) M's (multiples of 1000K) or G's (mul‐
     tiples of 1000M). This option is only relevant for the -L command.
    
     --line-numbers
     When listing rules, add line numbers to the beginning of each rule, corresponding to that rule's position in the chain.
    
     --modprobe=command
     When adding or inserting rules into a chain, use command to load any necessary modules (targets, match extensions, etc).
