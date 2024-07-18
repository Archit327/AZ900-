

Description: Lsass launched a child process. If this child process is unexpected, it might indicate malware hijacked lsass to evade detection or dump credentials. Investigate the process tree.
Customer ID: e2d8812954974296b4acb97d46bc082d
Full detection details: https://falcon.crowdstrike.com/activity-v2/detections/e2d8812954974296b4acb97d46bc082d:ind:806ccf82c5cc45898b2ae90ade7ccd4b:20334328744020-10231-331801104?_cid=g03000bwcxvcr67zewjehjcbdoqopt5i
Detected: Jul. 13, 2024 21:01:56 local time, (2024-07-13 21:01:56 UTC)
Host name: AZE-FS-ORG-P2
Agent ID: 806ccf82c5cc45898b2ae90ade7ccd4b
File name: WerFault.exe
File path: \Device\HarddiskVolume3\Windows\System32\WerFault.exe
Command line: C:\windows\system32\WerFault.exe -u -p 760 -s 2260
SHA 256: 906687a28e3e29ce712cb9a012f3b6d88872d24ff869111df306d8289f754212
MD5 Hash: bac8f80ee8da47374f2ba522c0d4a4c1
Platform: Windows
IP address: 20.85.213.225
User name: AZE-FS-ORG-P2$




{
  "incident_id": "inc:806ccf82c5cc45898b2ae90ade7ccd4b:a75894337650497daedc191497ce625a",
  "incident_type": 1,
  "cid": "e2d8812954974296b4acb97d46bc082d",
  "host_ids": [
    "806ccf82c5cc45898b2ae90ade7ccd4b"
  ],
  "hosts": [
    {
      "device_id": "806ccf82c5cc45898b2ae90ade7ccd4b",
      "cid": "e2d8812954974296b4acb97d46bc082d",
      "agent_load_flags": "1",
      "agent_local_time": "2024-06-15T23:16:35.970Z",
      "agent_version": "7.15.18511.0",
      "bios_manufacturer": "Microsoft Corporation",
      "bios_version": "Hyper-V UEFI Release v4.1",
      "config_id_base": "65994753",
      "config_id_build": "18511",
      "config_id_platform": "3",
      "external_ip": "20.85.213.225",
      "hostname": "AZE-FS-ORG-P2",
      "first_seen": "2023-06-17T06:23:01Z",
      "last_login_timestamp": "2024-07-12T23:01:18Z",
      "last_login_user": "ASathish",
      "last_seen": "2024-07-13T20:35:14Z",
      "local_ip": "10.204.8.34",
      "mac_address": "00-0d-3a-4e-77-f3",
      "machine_domain": "ads.mhainc.com",
      "major_version": "10",
      "minor_version": "0",
      "os_version": "Windows Server 2019",
      "ou": [
        "Production",
        "FLO",
        "_Servers"
      ],
      "platform_id": "0",
      "platform_name": "Windows",
      "product_type": "3",
      "product_type_desc": "Server",
      "site_name": "AzureEast",
      "status": "normal",
      "system_manufacturer": "Microsoft Corporation",
      "system_product_name": "Virtual Machine",
      "groups": [
        "3f6f050fef0d4315832c26d58c3b4bd0",
        "fc56a3ed496647c5a744857f2a179f96"
      ],
      "modified_timestamp": "2024-07-13T20:36:01Z",
      "instance_id": "0298375f-3289-4247-9212-b85f36ba005f",
      "service_provider": "AZURE",
      "service_provider_account_id": "e8328d3b-7c5e-4aa5-b321-eeb887f1fc6b"
    }
  ],
  "created": "2024-07-13T21:01:56Z",
  "start": "2024-07-13T21:01:55Z",
  "end": "2024-07-13T21:01:55Z",
  "state": "closed",
  "email_state": "START",
  "assigned_to_name": "eppteam@crowdstrike.com",
  "status": 40,
  "tactics": [
    "Defense Evasion"
  ],
  "techniques": [
    "Process Injection"
  ],
  "objectives": [
    "Keep Access"
  ],
  "modified_timestamp": "2024-07-13T23:02:25.007492834Z",
  "tags": [
    "False Positive"
  ],
  "fine_score": 13
}