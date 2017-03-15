# Splunk_Signal
Splunk Signal is a repository that contains the SIOS iQ integration with Splunk

# Instructions
To integrate the siosiq_alert_handler script with Splunk, follow these steps:

1) install the SIOS iQ Signal (SDK) on the splunk indexer system
   (copy the contents of the "python" sdk directory to /root/sdk)

2) configure SDK with your SIOS iQ appliance host information according to
   instructions in the SDK

3) if SDK is stored somewhere else, then modify siosiq_alert_handler path
   setting (change from /root/sdk to your location)

4) copy siosiq_alert_handler script to $SPLUNK_HOME/bin/scripts directory

5) make sure siosiq_alert_handler is executable (chmod +x)

6) create or modify a Splunk alert that retrieves VM related events so that
   the alert query generates events with the following fields:
     uuid = the VMware UUID of the VM that the event occurred on
     _time = time the event occurred (in epoch time, i.e. seconds
             since 1/1/1970)
     sios_environment_id = the SIOS iQ ID of the environment that the VM is in
     sios_layer (optional, default 'Compute') = 'Compute' or 'Storage'
                                                depending on the type of event
     sios_description (optional) = any additional information that you want
                                   (will appear in the SIOS iQ UI with the
                                   event)

7) add a script action (trigger) to the alert and specify siosiq_alert_handler

8) wait for the alert query to run and check the results in the splunk logs:
     /opt/splunk/var/log/splunk/python.log - contains an INFO message for each
                                             script execution
     /opt/splunk/var/log/splunk/splunkd.log - may contain an ERROR message if
                                              the script fails for some reason
     /var/log/syslog - siosiq_alert_handler logs to /var/log/syslog (set log
                       level to logging.DEBUG in siosiq_alert_handler to see
                       more logging information)
