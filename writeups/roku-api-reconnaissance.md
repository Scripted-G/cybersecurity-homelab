# Roku Device API Reconnaissance and Interaction

## Overview
Conducted reconnaissance and interaction with a Roku streaming device to demonstrate IoT API discovery, enumeration, and remote control capabilities. This exercise showcases how networked devices expose APIs that can be discovered and interacted with using standard tools.

## Objective
- Discover and identify Roku device on network
- Enumerate available API endpoints
- Test device security controls
- Demonstrate remote control via API
- Document API interaction techniques

## Lab Environment
- **Environment Type:** Personal home network (authorized testing)
- **Attack Platform:** Pop!_OS workstation with Nmap and curl
- **Target Device:** Roku 3920RW streaming player

## Tools Used
- **Nmap** - Network scanning and service detection
- **curl** - HTTP/API interaction and testing
- **Roku ECP (External Control Protocol)** - Documented API for Roku devices

## Methodology

### Phase 1: Device Discovery

**Network scan to identify Roku device:**
```bash
nmap [NETWORK_RANGE]
```

**Results:**
- Device identified with port 7000/tcp open (RTSP - AirTunes service)
- Initial scan showed limited port information
- Required targeted service enumeration

### Phase 2: Service Enumeration

**Test Roku ECP API availability:**
```bash
curl http://[ROKU_IP]:8060
```

**Response:**
Successfully retrieved device XML information including:
- Device type: Roku Streaming Player
- Model: 3920RW
- Manufacturer: Roku
- Available services: ECP (External Control Protocol) and DIAL

### Phase 3: API Discovery

**Query installed applications:**
```bash
curl http://[ROKU_IP]:8060/query/apps
```

**Key Finding:** Device returned complete list of installed applications, indicating that **Limited Mode was disabled**.

**Security Note:** Roku devices have a "Limited Mode" setting that blocks external control commands. In this case, Limited Mode was not enabled, allowing full API interaction.

**Applications enumerated (sample):**
- Netflix (ID: 12)
- YouTube (ID: 837)
- Prime Video (ID: 13)
- Hulu (ID: 2285)
- And 50+ additional streaming apps

### Phase 4: Active Application Detection

**Query currently active application:**
```bash
curl http://[ROKU_IP]:8060/query/active-app
```

**Result:**
```xml
<active-app>
    <app id="562859" type="home">Home</app>
</active-app>
```

Device was on the Home screen (app ID 562859).

### Phase 5: Remote Control Testing

**Test 1: Launch Application**

Attempted to remotely launch YouTube application:
```bash
curl -d '' http://[ROKU_IP]:8060/launch/837
```

**Verification:**
```bash
curl http://[ROKU_IP]:8060/query/active-app
```

**Result:** ✅ **Successful** - YouTube launched remotely
```xml
<active-app>
    <app id="837" type="appl">YouTube</app>
</active-app>
```

**Test 2: Navigation Control**

Sent Home button command to return to main menu:
```bash
curl -d '' http://[ROKU_IP]:8060/keypress/Home
```

**Result:** ✅ **Successful** - Device returned to Home screen

## Findings

### API Endpoints Discovered

**Device Information:**
- `GET http://[ROKU_IP]:8060` - Returns device XML with model, manufacturer, services
- `GET http://[ROKU_IP]:8060/query/device-info` - Detailed device information

**Application Control:**
- `GET http://[ROKU_IP]:8060/query/apps` - List all installed applications
- `GET http://[ROKU_IP]:8060/query/active-app` - Get currently running app
- `POST http://[ROKU_IP]:8060/launch/[APP_ID]` - Launch specific application

**Remote Control:**
- `POST http://[ROKU_IP]:8060/keypress/Home` - Home button
- `POST http://[ROKU_IP]:8060/keypress/Select` - Select/OK button
- `POST http://[ROKU_IP]:8060/keypress/Up/Down/Left/Right` - Navigation
- `POST http://[ROKU_IP]:8060/keypress/Back` - Back button
- `POST http://[ROKU_IP]:8060/keypress/Play` - Play/Pause

### Security Assessment

**Vulnerabilities Identified:**
1. ✅ **Limited Mode Disabled** - Device accepts remote control commands
2. ✅ **No Authentication Required** - API endpoints accessible without credentials
3. ✅ **Network Accessible** - Any device on local network can control Roku
4. ✅ **Information Disclosure** - Device model, serial number, and app list exposed

**Risk Level:** Low to Medium (Local Network Only)

**Mitigation:**
- Device only accepts commands from local network (not exposed to internet)
- Roku's ECP is designed for legitimate remote control applications
- Limited Mode can be enabled to block external control
- Network segmentation would prevent unauthorized access

### Attack Scenarios (Hypothetical)

If an attacker gained access to the local network, they could:
1. **Information Gathering** - Enumerate installed apps to profile user interests
2. **Annoyance Attacks** - Launch apps, change channels, disrupt viewing
3. **Social Engineering** - Display content to manipulate occupants
4. **Surveillance Preparation** - Identify streaming habits and schedules

**Reality Check:** These scenarios require local network access, making them low probability in most environments.

## Technical Skills Demonstrated

### Reconnaissance Skills
1. **Network Scanning** - Identified IoT device via port scanning
2. **Service Enumeration** - Discovered non-standard ports (8060) through testing
3. **API Discovery** - Identified undocumented API endpoints through experimentation
4. **Protocol Analysis** - Understanding HTTP-based control protocols

### API Interaction
1. **RESTful API Testing** - GET and POST requests to control endpoints
2. **XML Parsing** - Interpreting device responses in XML format
3. **Command Injection** - Sending control commands via API
4. **State Verification** - Confirming successful command execution

### Security Analysis
1. **Access Control Evaluation** - Identified lack of authentication
2. **Security Control Testing** - Discovered Limited Mode was disabled
3. **Attack Surface Assessment** - Documented exposed endpoints and capabilities
4. **Risk Assessment** - Evaluated real-world exploitability

## Key Takeaways

### Lessons Learned

1. **IoT Devices Expose APIs** - Many "smart" devices have undocumented or poorly documented APIs
2. **Security Features Must Be Enabled** - Limited Mode exists but isn't enabled by default
3. **Local Network Access = Control** - Physical network security is critical for IoT
4. **Documentation Matters** - Roku's ECP is actually documented, making it easy to interact with
5. **Legitimate vs. Malicious Use** - Same API used for remote apps and potential attacks

### Real-World Applications

**SOC Analyst Skills:**
- IoT device identification and inventory
- Understanding device communication patterns
- Recognizing normal vs. suspicious API usage

**Penetration Testing Skills:**
- API enumeration and discovery
- Authentication bypass (in this case, lack of auth)
- Remote control and manipulation of devices
- Documentation of findings and recommendations

**Network Security:**
- Understanding IoT attack surfaces
- Importance of network segmentation
- Evaluating device security controls

## Remediation Recommendations

**For This Device:**
1. ✅ Enable Limited Mode in Roku settings to block external control
2. ✅ Verify device is not exposed to internet (port forwarding, UPnP)
3. ✅ Consider VLAN segmentation for IoT devices
4. ⚠️ Current state acceptable for home use with trusted network

**General IoT Security Best Practices:**
1. **Network Segmentation** - Separate IoT devices from critical systems
2. **Regular Updates** - Keep device firmware current
3. **Disable Unnecessary Features** - Turn off remote control if not needed
4. **Monitor Network Traffic** - Watch for unusual device behavior
5. **Access Control** - Use strong WiFi passwords and WPA3 encryption

## Ethical Considerations

**Authorization:**
- ✅ All testing performed on personally owned device
- ✅ Testing conducted on private network with full authorization
- ✅ No attempts to access devices outside of controlled environment

**Responsible Use:**
- This API is designed for legitimate remote control applications
- Roku provides official apps that use this same API
- Testing demonstrates security implications, not malicious intent

## Conclusion

This reconnaissance exercise successfully demonstrated how IoT devices expose APIs that can be discovered through enumeration and interacted with using standard tools. The Roku ECP API provides comprehensive remote control capabilities with minimal security controls, relying primarily on network-level access restrictions.

**Key Success Metrics:**
- ✅ Successfully identified device on network
- ✅ Discovered and documented API endpoints
- ✅ Demonstrated remote control capabilities
- ✅ Evaluated security controls (Limited Mode)
- ✅ Assessed real-world risk and mitigation strategies

**Skills Practiced:**
- Network reconnaissance
- API discovery and enumeration
- HTTP/REST API interaction
- IoT device security assessment
- Professional security documentation

This exercise demonstrates practical skills applicable to SOC Analyst and Penetration Testing roles, showing the ability to discover, enumerate, and assess networked devices in a methodical and documented manner.

---

**Date:** November 2025  
**Environment:** Personal Home Lab Network  
**Assessment Duration:** ~1 hour  
**Tools Used:** Nmap, curl, Roku ECP API

## References

- [Roku External Control Protocol Documentation](https://developer.roku.com/docs/developer-program/debugging/external-control-api.md)
- [Roku ECP Guide](https://developer.roku.com/docs/developer-program/debugging/external-control-api.md)
- [OWASP IoT Security Testing Guide](https://owasp.org/www-project-iot-security-testing-guide/)

## Disclaimer

This security assessment was conducted on a personally owned device within a private network environment with full authorization. All testing was performed for educational purposes to demonstrate network security assessment skills applicable to professional cybersecurity roles.

**Important:** Unauthorized access to devices or networks is illegal. This documentation is for educational and portfolio purposes only. Always obtain explicit written permission before conducting security assessments on any device or network you do not own.
