1) Create a [Script Module](https://octopus.com/docs/administration/script-modules) with the following code

```powershell
$machineName = $OctopusParameters['Octopus.Machine.Name']

set-OctopusVariable -name "MachineName" -value $machinename
```

2) Import the script module from (1) into your project.

3) Create an Email step with the below code:

```
#{each step in Octopus.Step}
	<b>Step:#{step}</b>
	#{unless Octopus.Action[#{step}].Output.MachineName}
		<li>Octopus Server</li>
	#{/unless}
	#{unless Octopus.Action[#{step}].RunOnServer}
		#{each machine in Octopus.Action[#{step}].Output}		
			<li>#{machine.MachineName}</li>		
		#{/each}	
	#{/unless}
	#{if Octopus.Action[#{step}].RunOnServer}
		<li>Octopus Server</li>
	#{/if}
	<div></div>
#{/each}
```

