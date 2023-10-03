
#### Jenkins Infrastructure
  Master Server:
    - Controls Pipeline
    - Schedules job

  Agents/Minions:
    - Performs the build

  1. Commit triggers the pipleine
  2. Agent selected based on config labels
  3. Agent runs the build

Agent types:
  Permanent agents
	  - Dedicated servers for running jobs
	  eg. linux, windows
  Cloud Agents:
	  - Dynamic agent spun up on demand
	  eg. Docker, kubernetes, aws

