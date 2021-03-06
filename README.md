# esb-inbound-dbevent

The DB Event inbound endpoint allows you to capture the data change via WSO2 ESB.

##Build

mvn clean install

###How You Can Contribute

You can create a third party connector and publish in WSO2 Connector Store.

https://docs.wso2.com/display/ESBCONNECTORS/Creating+a+Third+Party+Connector+and+Publishing+in+WSO2+Connector+Store

Pre-requisites:

 - Maven 3.x
 - Java 1.6 or above
 - The JDBC driver e.g:mysql-connector-java-5.1.38-bin.jar.

Tested Platform: 

 - Microsoft WINDOWS V-7
 - Mac OSX 10.11.6
 - wso2ei-6.1.1
 - Java 1.7

1. To use the DB Event inbound endpoint, you need to download the inbound org.apache.synapse.dbevent.poll-1.0.0.jar from https://store.wso2.com and copy the jar to the <EI_HOME>/lib directory.

2. Place the mysql-connector-java-5.1.38-bin.jar into the directory <EI_HOME>/lib.

2. Configuration:

<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                 name="customIE"
                 sequence="request"
                 onError="fault"
                 class="org.wso2.carbon.inbound.poll.dbeventlistener.DBEventPollingConsumer"
                 suspend="false">
   <parameters>
      <parameter name="inbound.behavior">polling</parameter>
      <parameter name="interval">10000</parameter>
      <parameter name="sequential">true</parameter>
      <parameter name="coordination">true</parameter>
      <parameter name="driverName">com.mysql.jdbc.Driver</parameter>
      <parameter name="url">jdbc:mysql://localhost/test</parameter>
      <parameter name="username">root</parameter>
      <parameter name="tableName">CDC_CUSTOM</parameter>
      <parameter name="filteringCriteria">byLastUpdatedTimestampColumn</parameter>
      <parameter name="filteringColumnName">LAST_UPDATED_DATE_TIME</parameter>
      <parameter name="primaryKey">ID</parameter>
      <parameter name="connectionValidationQuery">SELECT 1</parameter>
      <parameter name="registryPath">customIE/timestamp</parameter>
   </parameters>
</inboundEndpoint>

1. driverName                - The class name of the database driver.
2. url	                      - The JDBC URL of the database.
3. username                  - The user name used to connect to the database.
4. password                  - The password used to connect to the database.
5. tableName                 - The name of the table to capture the change.
6. filteringCriteria         - It can be one of these byLastUpdatedTimestampColumn or byBooleanColumn or deleteAfterPoll.
7. filteringColumnName       - The actual name of table column. It must be set if the filteringCriteria has the value 'byLastUpdatedTimestampColumn' or 'byBooleanColumn'.
8. primaryKey                - The primary key column name.
9. connectionValidationQuery - The query to check the availability of the connection.
10. registryPath             - The registry path of the timestamp. This will be used to retrieve the records when the filteringCriteria has the value 'byLastUpdatedTimestampColumn'.
