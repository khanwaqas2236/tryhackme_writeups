## this script send data on auto to the recipient if u stop it running you have to start it again within the time to not send data ##


import os
import time
import threading
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders

# Configuration
CHECK_INTERVAL = 10  # Check every 10 seconds (reduced for testing)
TRIGGER_TIMEOUT = 60  # 60 seconds without check-in triggers the data release
TRIGGER_FILE = "last_checkin.txt"
ALERT_EMAIL = "salmantahir075@gmail.com"
ALERT_PASSWORD = "wlsv tdrc egra svhl"
RECIPIENT_EMAIL = "salmantahir075@gmail.com"
DATA_FILE = "data_dump.zip"
SMTP_SERVER = "smtp.gmail.com"
SMTP_PORT = 587


def send_alert():
    print("[+] Triggering data release...")
    try:
        # Prepare the email
        msg = MIMEMultipart()
        msg["From"] = ALERT_EMAIL
        msg["To"] = RECIPIENT_EMAIL
        msg["Subject"] = "Dead Man's Switch - Data Released"
        msg.attach(MIMEText("The attached file contains the released data.", "plain"))

        # Attach the data file
        print("[+] Attaching data file...")
        with open(DATA_FILE, "rb") as file:
            part = MIMEBase("application", "octet-stream")
            part.set_payload(file.read())
            encoders.encode_base64(part)
            part.add_header(
                "Content-Disposition",
                f"attachment; filename= {os.path.basename(DATA_FILE)}",
            )
            msg.attach(part)

        # Send the email
        print("[+] Connecting to SMTP server...")
        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        print("[+] Logging in...")
        server.login(ALERT_EMAIL, ALERT_PASSWORD)
        print("[+] Sending email...")
        server.sendmail(ALERT_EMAIL, RECIPIENT_EMAIL, msg.as_string())
        server.quit()
        print("[+] Data sent successfully.")
    except Exception as e:
        print(f"[!] Failed to send data: {e}")


def monitor_switch():
    # Delay to allow initial check-in to settle
    print("[+] Monitoring will start in 5 seconds to allow initial check-in...")
    time.sleep(5)
    while True:
        try:
            # Check if the trigger file exists
            if os.path.exists(TRIGGER_FILE):
                last_checkin = os.path.getmtime(TRIGGER_FILE)
                current_time = time.time()
                time_remaining = TRIGGER_TIMEOUT - (current_time - last_checkin)
                if time_remaining > 0:
                    print(f"[+] Time remaining before data release: {int(time_remaining)} seconds")
                else:
                    print("[+] No check-in detected. Triggering alert...")
                    send_alert()
                    break
            else:
                print("[!] No check-in file found. Creating initial check-in.")
                check_in()  # Create the initial check-in file
        except Exception as e:
            print(f"[!] Error during monitoring: {e}")
        time.sleep(CHECK_INTERVAL)


def check_in():
    # Update the check-in timestamp
    with open(TRIGGER_FILE, "w") as file:
        file.write(str(time.time()))
    print("[+] Check-in successful.")


if __name__ == "__main__":
    # Create an initial check-in if the file is missing
    if not os.path.exists(TRIGGER_FILE):
        print("[+] Creating initial check-in...")
        check_in()
    else:
        print("[+] Existing check-in file found. Resuming monitoring.")

    # Start the monitoring thread
    monitor_thread = threading.Thread(target=monitor_switch)
    monitor_thread.start()

    # Simulate periodic check-ins (for testing)
    while True:
        check_in()
        time.sleep(10)  # Check in every 10 seconds (reduced for testing)
