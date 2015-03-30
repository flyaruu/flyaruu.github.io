---
title: Docker API JSON example
layout: page
permalink: /info_json
---
{% highlight json %}
{
  "AppArmorProfile" : "",
  "Args" : [ "mysqld" ],
  "Config" : {
    "AttachStderr" : true,
    "AttachStdin" : false,
    "AttachStdout" : true,
    "Cmd" : [ "mysqld" ],
    "CpuShares" : 0,
    "Cpuset" : "",
    "Domainname" : "",
    "Entrypoint" : [ "/entrypoint.sh" ],
    "Env" : [ "MYSQL_ROOT_PASSWORD=mysecretpassword", "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin", "MYSQL_MAJOR=5.6", "MYSQL_VERSION=5.6.23" ],
    "ExposedPorts" : {
      "3306/tcp" : { }
    },
    "Hostname" : "e56f7ca2438f",
    "Image" : "mysql",
    "MacAddress" : "",
    "Memory" : 0,
    "MemorySwap" : 0,
    "NetworkDisabled" : false,
    "OnBuild" : null,
    "OpenStdin" : false,
    "PortSpecs" : null,
    "StdinOnce" : false,
    "Tty" : false,
    "User" : "",
    "Volumes" : {
      "/var/lib/mysql" : { }
    },
    "WorkingDir" : ""
  },
  "Created" : "2015-03-26T15:11:04.391068513Z",
  "Driver" : "aufs",
  "ExecDriver" : "native-0.2",
  "ExecIDs" : null,
  "HostConfig" : {
    "Binds" : null,
    "CapAdd" : null,
    "CapDrop" : null,
    "ContainerIDFile" : "",
    "Devices" : [ ],
    "Dns" : null,
    "DnsSearch" : null,
    "ExtraHosts" : null,
    "IpcMode" : "",
    "Links" : null,
    "LxcConf" : [ ],
    "NetworkMode" : "bridge",
    "PidMode" : "",
    "PortBindings" : { },
    "Privileged" : false,
    "PublishAllPorts" : true,
    "ReadonlyRootfs" : false,
    "RestartPolicy" : {
      "MaximumRetryCount" : 0,
      "Name" : ""
    },
    "SecurityOpt" : null,
    "VolumesFrom" : null
  },
  "HostnamePath" : "/mnt/sda1/var/lib/docker/containers/e56f7ca2438f6b84e06ee1439049b1c6b46a5dafc97708d3da92db906f66ce43/hostname",
  "HostsPath" : "/mnt/sda1/var/lib/docker/containers/e56f7ca2438f6b84e06ee1439049b1c6b46a5dafc97708d3da92db906f66ce43/hosts",
  "Id" : "e56f7ca2438f6b84e06ee1439049b1c6b46a5dafc97708d3da92db906f66ce43",
  "Image" : "e93afb6a83e9df759e58f73804d2a7531ade4babeae6a3961e636885d97c2173",
  "MountLabel" : "",
  "NetworkSettings" : {
    "Bridge" : "docker0",
    "Gateway" : "172.17.42.1",
    "GlobalIPv6Address" : "",
    "GlobalIPv6PrefixLen" : 0,
    "IPAddress" : "172.17.0.47",
    "IPPrefixLen" : 16,
    "IPv6Gateway" : "",
    "LinkLocalIPv6Address" : "fe80::42:acff:fe11:2f",
    "LinkLocalIPv6PrefixLen" : 64,
    "MacAddress" : "02:42:ac:11:00:2f",
    "PortMapping" : null,
    "Ports" : {
      "3306/tcp" : [ {
        "HostIp" : "0.0.0.0",
        "HostPort" : "49212"
      } ]
    }
  },
  "Path" : "/entrypoint.sh",
  "ProcessLabel" : "",
  "ResolvConfPath" : "/mnt/sda1/var/lib/docker/containers/e56f7ca2438f6b84e06ee1439049b1c6b46a5dafc97708d3da92db906f66ce43/resolv.conf",
  "RestartCount" : 0,
  "State" : {
    "Error" : "",
    "ExitCode" : 0,
    "FinishedAt" : "0001-01-01T00:00:00Z",
    "OOMKilled" : false,
    "Paused" : false,
    "Pid" : 7174,
    "Restarting" : false,
    "Running" : true,
    "StartedAt" : "2015-03-26T15:11:04.798228845Z"
  },
  "Volumes" : {
    "/var/lib/mysql" : "/mnt/sda1/var/lib/docker/vfs/dir/1afe0ba6b27b8b3f714e72ecedeb77a828838abe9908f4aa889d65fa2727ae01"
  },
  "VolumesRW" : {
    "/var/lib/mysql" : true
  }
}
{% endhighlight %}