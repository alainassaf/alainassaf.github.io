---
layout: post
title: End User Experience Monitoring Statistics Explained
date: 2011-04-06
tags: [Citrix, Documentation, EdgeSight, Repost, Reporting, Monitoring]
---
<a href="http://web.archive.org/web/20220707120540/https://support.citrix.com/article/CTX114495/edgesight-end-user-experience-monitoring-statistics-explained" target="_blank"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;float:left;padding-top:0;border:0;margin:0 15px 0 5px;" title="index" src="/assets/img/icons/es-logo.jpg" alt="" width="134" height="134" align="left" border="0" />
I dive into many tables and views that are part of the EdgeSight database to produce custom reports for my co-workers and superiors.  Many of the metrics are easy to understand and many more are a little obtuse.  Here is a great CTX article that fully explains the metrics related to End User Experience Monitoring.

<p style="text-align:left;"><em>Source: <a href="http://web.archive.org/web/20220707120540/https://support.citrix.com/article/CTX114495/edgesight-end-user-experience-monitoring-statistics-explained">CTX114495 - End User Experience Monitoring Statistics Explained - Citrix Knowledge Center</a></em></p>
<p style="text-align:left;"><em>Updated:</em> <a href="http://web.archive.org/web/20190504011952/https://support.citrix.com/help/edgesight/EdgeSight-Reporting.pdf">End User Experience Monitoring Data</a></p>
<span class="stellent-heading2"><strong>Summary</strong></span>

This article describes the metrics for the Session Experience Monitoring Service (SEMS).

<span class="stellent-heading2"><strong>Background</strong></span>

EdgeSight for Presentation Server provides highly granular session experience monitoring data collected through Presentation Server and Presentation Server Client instrumentation. Data is collected through the SEMS. The following metrics are stored in the EdgeSight Agent database and can be displayed through the Presentation Server User Summary remote report.

<strong>Note</strong>: Data that is uploaded to the server and displayed in historical reports is listed in the Historical Report Counters and data fields.

The collection of SEMS data is dependent on the version of the EdgeSight Agent, Presentation Server, and Presentation Server Client installed, as described in the Session Experience Monitoring Metrics help topic under Working with Reports within the EdgeSight Server Console.

<strong>Note</strong>: Presentation Server 4.5 Enterprise or Platinum Edition or later is required to capture SEMS data.

This process is described below:

1. The Presentation Server Client and server running Presentation Server negotiate capabilities during session creation.

2. The launch mechanism and metrics are captured by Web Interface/Program Neighborhood Agent and recorded in the ICA file downloaded to the client.

3. The client and server metrics are sent to the SEMS (SemsService.exe) and are stored in memory.

4. The End User Experience Monitoring (EUEM) metrics are published to EdgeSight Agent and stored locally in the Firebird database along with other metrics.

<strong>Note</strong>: All time measurements are in milliseconds unless otherwise noted.

<span class="stellent-heading3"><strong>Session performance metrics:</strong></span>

**AVG_NETWORK_LATENCY**
Network latency is the detected network latency between the Presentation Server Client device and the server running Presentation Server. Unlike the ICA round trip metric, the network round trip is largely independent of processing time on the client or server.

**AVG_ROUND_TRIP_TIME**
The ICA round trip time. The average time interval measured at the client between the first step (user action) and the last step (graphical response is displayed).

**INPUT_BANDWIDTH_USED**
The actual bandwidth consumed on the network (Presentation Server Client to Presentation Server) in bits per second. This measurement is taken at the server end of the connection by counting the actual bytes that flow to and from the server running Presentation Server for each user’s session. It measures traffic on the wire (after optimization and compression has taken place). The network bandwidth used is averaged across the ICA round trip check period (by default, the Presentation Server Client initiates an ICA round trip check every 15 seconds).

**OUTPUT_BANDWIDTH_USED**
The actual bandwidth consumed on the network (Presentation Server to Presentation Server Client) in bits per second. This measurement is taken at the server end of the connection, counting the actual bytes that flow both to and from the server running Presentation Server for each user’s session. It measures traffic on the wire (after optimization and compression has taken place). The network bandwidth used is averaged across the ICA round trip check period.

<span class="stellent-heading3"><strong>Session performance data (ICA round trip):</strong></span>

<strong>Note</strong>: Metrics on the duration of a portion of the handling of a graphical frame during the measurement of ICA roundtrip time (EUEM trigger, frame cut, frame send, first draw, and Winstation Driver trigger) are included in this report. A frame is the unit of graphical transfer from the server to the client. Note that depending on the trigger for the graphical frame, not all of these metrics will be present for each ICA round trip.

**SESSION_START**
The time that the session was started.

**SESSION_ID**
Session identifier.

**CLIENT_NAME**
Name of the client associated with the session.

**CLIENT_ADDRESS**
Address of the client associated with the session.

**CLIENT_VERSION**
Version of the client associated with the session.

**DOMAIN_NAME**
Name of the domain to which the user belongs.

**ACCOUNT_NAME**
The user account name.

**PROTOCOL_TYPE**
The type of protocol used for the session.

**WD_TRIGGER_ROUND_TRIP**
The time from the start of the frame until frame handling is complete. This metric is collected when the trigger for the frame is a timeout in the Winstation Driver.

**EUEM_TRIGGER_ROUND_TRIP**
The time from the start of the frame until frame handling is complete. This metric is collected when an ICA round trip measurement occurs.

**ICA_NETWORK_LATENCY**
The detected network latency between the Presentation Server Client device and the server running Presentation Server.

**FIRST_DRAW_ROUND_TRIP**
The time from the start of the frame until frame handling is complete. This metric is collected when the frame trigger is the receipt of a graphical operation from an application.

**ICA_INPUT_BANDWIDTH_USED**
The actual bandwidth consumed on the network (Presentation Server Client to Presentation Server) in bits per second.

**INPUT_BANDWIDTH_AVAILABLE**
The bandwidth available on the network (Presentation Server Client to Presentation Server) in bits per second.

**ICA_ROUND_TRIP**
The time interval measured at the client between the first step (user action) and the last step (graphical response displayed). This metric can be thought of as a measurement of the screen lag that a user experiences while interacting with an application hosted in a session on a server running Presentation Server.

**FRAME_CUT_ROUND_TRIP**
The time from the start of the frame until the point at which the frame is complete.

**FRAME_SEND_ROUND_TRIP**
The time from the start of the frame until the completion of sending frame data to the client.

**ICA_OUTPUT_BANDWIDTH_USED**
The actual bandwidth consumed on the network (Presentation Server to Presentation Server Client) in bits per second.

**OUTPUT_BANDWIDTH_AVAILABLE**
The bandwidth available on the network (Presentation Server to Presentation Server Client) in bits per second. A value of 0 indicates that no data was available.

**SYSTEMTIME**
The time when the data was collected, as recorded on the system running the EdgeSight Agent.

<span class="stellent-heading3"><strong>Session client startup data:</strong></span>

**LOGON_TIME**
The time that the user logged on to start the session.

**APP_NAME**
The name of the published application.

**LAUNCH_TYPE**
The type of session launch. The values are listed below:

0—Unknown
1—Obsolete (This could be a captured ICA file being used through file-type association.)
2—Web Interface
3—Program Neighborhood
4—Program Neighborhood Agent
5—Web portal (Advanced Access Control or MetaFrame Secure Access Manager)

**LAUNCH_TYPE_ID**
Launch type identifier (0-5), indicating a type of session launch as shown above under LAUNCH_TYPE.

**SESSION_ID**
The session identifier.

**SESSION_SHARED_FLAG**
A flag indicating whether a pre-existing session is being shared for this application launch.

**SCCD - STARTUP_CLIENT**
This is the high-level client connection startup metric. It starts as close as possible to the time of the request (mouse click) and ends when the ICA connection between the client device and server running Presentation Server has been established. In the case of a shared session, this duration will normally be much smaller, as many of the setup costs associated with the creation of a new connection to the server are not incurred.

**CFDCD - CONFIG_FILE_DOWNLOAD_CLIENT**
The time it takes to get the configuration file from the XML server.

**BUCC - BACKUP_URL_CLIENT_COUNT**
The number of backup URL retries before success. Note that this is the only start-up metric that is a count of attempts, rather than a duration.

**AECD - APPLICATION_ENUM_CLIENT**
The time it takes to get the list of applications.

**COCD - CREDENTIALS_OBTENTION_CLIENT**
The time it takes to get the user credentials. Note that COCD is only measured when credentials are entered manually by the user.

**IFDCD - ICA_FILE_DOWNLOAD_CLIENT**
The time it takes the client to download the ICA file from the Web server for Program Neighborhood Agent or Web Interface.

**NRWD - NAME_RESOLUTION_WEB_SERVER**
The time it takes the XML Service to resolve the name of a published app to a presentation server address. This metric is collected when the application is launched through the Program Neighborhood Agent or Web Interface.

**RECONNECT_ENUM_CLIENT**
The time it takes a client to get a list of reconnections.

**RECONNECT_ENUM_WEB_SERVER**
The time it takes the Web Interface to get the list of reconnections from the XML Service.

**TRWD - TICKET_RESPONSE_WEB_SERVER**
The time it takes to get a ticket (if required) from the STA server or XML Service. This metric is collected when the application is launched via the Program Neighborhood Agent or Web Interface.

**LPWD - LAUNCH_PAGE_WEB_SERVER**
The time it takes to process the launch page (launch.aspx) on the Web Interface server.

**SCD - SESSION_CREATION_CLIENT**
New session creation time, from the moment wfica32.exe is launched to when the connection is established.

**NRCD - NAME_RESOLUTION_CLIENT**
The time it takes the XML Service to resolve the name of a published application to an IP address. This metric is only collected for new sessions, and only if the ICA file does not specify a connection to a Presentation Server with the IP address already provided.

**SLCD - SESSION_LOOKUP_CLIENT**
The time it takes to query every ICA session to host the requested published application. The check is performed in the client to determine whether the application launch request can be handled by an existing session. A different method is used depending on whether the sessions are new or shared.

<span class="stellent-heading3"><strong>Session Server Startup Data:</strong></span>

**LOGON_TIME**
The time that the user logged on to start the session.

**SESSION_ID**
The session identifier.

**SESSION_CREATION_STARTED**
The time that session creation started.

**SESSION_CREATION_ENDED**
The time that session creation was completed.

**SSSD - SESSION_STARTUP_SERVER**
This is the high-level server connection startup metric. This includes the time spent on the Presentation Server performing the entire startup operation. In the event of an application starting in a shared session, this metric is expected to be much smaller, as starting a completely new session involves potentially high-cost tasks, such as profile loading and logon script execution.

**CASD - CREDENTIALS_AUTH_SERVER**
The time spent on the server authenticating the user credentials.

**CONSD - CREDENTIALS_OBT_NETWORK_SVR**
The time spent by the server performing network operations to obtain credentials for the user. This only applies to a Security Support Provider Interface (SSPI) logon (a form of pass-through authentication where the client device is a member of the same domain as the server and Kerberos tickets are passed in place of manually entered credentials).

**COSD - CREDENTIALS_OBT_SERVER**
This is also called COSD. It is the time taken for the server to obtain the user credentials. This is only likely to be a significant amount of time if a manual logon is used and the server-side credentials dialog is displayed (or if a legal notice is displayed before the logon commences).

**PNCOSD - PNC_CREDENTIALS_OBT_SERVER**
The time taken for the server to cause the Program Neighborhood instance running on the client (Program Neighborhood Classic) to obtain the user credentials. This credentials dialog is displayed and managed by the client side, but the duration is measured on the server.

**DMSD - DEVICE_MAPPING_SERVER**
The time spent on the server mapping the user's client drives, devices, and ports.

**LSESD - LOGIN_SCRIPT_EXEC_SERVER**
The time spent on the server running the user's logon script(s).

**PCSD - PRINTER_CREATION_SERVER**
The time spent on the server synchronously mapping the user's client printers. If the configuration is set such that this printer creation is performed asynchronously, no value is recorded; it does not affect the completion of the session startup.

**PLSD - PROFILE_LOAD_SERVER**
The time spent on the server loading the user's profile.

**SCSD - SESSION_CREATION_SERVER**
The time spent on the server creating the session. The duration starts when the Presentation Server Client connection is opened and ends when authentication begins. This should not be confused with the overall Session Startup Server duration.

### Value for Value
If you received any value from reading this post, please help by becoming a [**supporter**](https://www.paypal.com/donate?hosted_button_id=73HNLGA2SGLLU).

*Thanks for reading,*  
*Alain*