# What is this?
This is a JVM flag for Minecraft to make it possible to hide <br>
`"[main/WARN]: Reference map 'mod-name.refmap.json' for mod-name.fabric.mixins.json could not be read. If this is a development environment you can ignore this message"` <br>
warning messages caused by some mods.

# Why?
I have many mods installed and nearly all of them gives these warning messages that are completely safe to ignore. I'm just sick of these messages filling the logs. <br>
My first idea was to make a Fabric mod but it wouldn't work since every mod starts after the warning messages. 

# How to use?
It should work properly on any Minecraft version, any mod loader and on server or client.

Simply add
```
-Dlog4j.configurationFile=https://nrmflag.github.io/jvm.xml
```
to your startup command.

## Example:
```
java -Xms512M -Xmx4G -Dlog4j.configurationFile=https://nrmflag.github.io/jvm.xml -jar server.jar
```

# Why this URL?
I don't have money to buy a domain that would shorten the url so I have to use Github Pages to shorten it and also give a detailed explanation.

# What does the "jvm.xml" contain?
```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="SysOut" target="SYSTEM_OUT">
            <PatternLayout pattern="[%d{HH:mm:ss}] [%t/%level]: %msg%n" />
            <RegexFilter regex=".*(If this is a development environment).*" onMatch="DENY" onMismatch="NEUTRAL"/>
        </Console>

        <RollingRandomAccessFile name="File" fileName="logs/latest.log" filePattern="logs/%d{yyyy-MM-dd}-%i.log.gz">
            <PatternLayout pattern="[%d{HH:mm:ss}] [%t/%level]: %msg%n" />
            <Policies>
                <TimeBasedTriggeringPolicy />
                <OnStartupTriggeringPolicy />
            </Policies>
            <RegexFilter regex=".*(If this is a development environment).*" onMatch="DENY" onMismatch="NEUTRAL"/>
        </RollingRandomAccessFile>
    </Appenders>

    <Loggers>
        <Root level="info">
            <AppenderRef ref="SysOut"/>
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>

```
