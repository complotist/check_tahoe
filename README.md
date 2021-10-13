### nagios/icinga check plugin for tahoe-lafs


#### services/tahoe.conf

	apply Service "tahoe-" for (tahoe => config in host.vars.tahoe) {
	  import "generic-service"
	  check_command = "tahoe"
	  display_name = "tahoe status for " + tahoe
	  check_interval = 180s
	  enable_notifications = false
	  command_endpoint = host.vars.client_endpoint
	  vars += config
	  assign where typeof(host.vars.tahoe) == Dictionary
	}

#### commands/tahoe.conf

	object CheckCommand "tahoe" {
	  command = [ PluginDir + "/check_tahoe" ]
	  arguments = {
	    "--hostname" = {
	      value = "$tahoe_hostname$"
	      description = "Hostname (default: 127.0.0.1)"
	    }
	    "--port" = {
	      value = "$tahoe_port$"
	      description = "tahoe port (default: 3456)"
	    }
	    "--stats" = {
	      value = "$tahoe_stats$"
	      description = "tahoe stats uri (default: statistics?t=json)"
	    }
	    "--cpu_warning" = {
	      value = "$tahoe_cpu_warning$"
	      description = "warning cpu threshold (default: 0.7)"
	    }
	    "--cpu_critical" = {
	      value = "$tahoe_cpu_critical$"
	      description = "critical cpu threshold (default: 0.9)"
	    }
	    "--disk_warning" = {
	      value = "$tahoe_disk_warning$"
	      description = "warning disk threshold (default: 10% free)"
	    }
	    "--disk_critical" = {
	      value = "$tahoe_disk_critical$"
	      description = "critical disk threshold (default: 5% free)"
	    }
	  }
	}

#### some-machine.conf

	  vars.tahoe = {
	    "tahoe" = {}
	    "tahoe-ila" = {
	      tahoe_hostname = "127.0.0.2"
	    }
	  }

