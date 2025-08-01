# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

### Changed

### Fixed


## [0.34.2 – 2025-08-01]
This release mainly fixes a bug in input/scp that made ssh raise an error when
closing a closed connection (Issue #3583).

A fix for config vars (Issue #3536) changes the way oxidized stores its
vars internally (symblos => strings). Libraries depending on oxidized internal
structures may have problem with this. oxidized-web was fixed in Release 0.17.1.

### Added
- Absolute time limit for a fetch job (default: 300 seconds) (@robertcheramy)

### Changed
- slackdiff: Attempt to join the channel if Errors::NotInChannel is encountered (@varesa)

### Fixed
- SSH raises error when closing a closed connection. Fixes #3583 (@ytti)
- Config vars will not fall back to less specific. Fixes #3536 (@ytti)
- input/scp: make common errors produce a warning, not a crashfile (@robertcheramy)
- input/scp: implement timeouts. Fixes #3590 (@robertcheramy, @ytti)
- model/mtrlrfs: add missing prompt (@R3thos)
- slackdiff: Respect the HTTP proxy configuration while uploading the file. Fixes #3534 (@varesa)
- logging (syslog): do not write two timestamps (Fixed in semanticlogger) (@robertcheramy)


## [0.34.1 - 2025-07-18]
This release contains small fixes and will include the new version of oxidized-web (0.17.0) in the docker container.

### Changed
- github: run ruby CI against ruby-head (@robertcheramy)

### Fixed
- input/ssh: hide Net::SSH errors and only display fatal logs unless input.debug = true. Fixes: #3574 (@robertcheramy)
- junos: fix unfrozen literal strings (@robertcheramy)
- spec/model: fix unfrozen literal strings and set a default prompt (@robertcheramy)


## [0.34.0 - 2025-07-15]
:warning: This release introduces a [new logging system](docs/Configuration.md#logging),
based on [semantic logger](https://logger.rocketjob.io/). The old configuration
(`log`, `syslog`) is still supported but obsolete and will be removed in a
future release, so be sure to migrate your configuration.

### Added
- add iosxr support to SyslogMonitor (@deesel)
- add junos: support show chassis cluster when SRX series (@shigechika)
- add nxos: support for complete hardware inventory (@garryshtern)
- ssh: support 'newline "string"' cfg block method to allow defining \r\n newline (@ytti)
- model for Netgate TNSR (@Vantomas)
- efos: New model for Brocade Enhanced Fabric OS. Fixes #3477 (@sorano, @cetjcm, @robertcheramy)
- output/file, output/git: clean node configurations which are not listed in the
  source anymore. Fixes: #1805 (@robertcheramy)
- sixwind: New model to support 6WIND Virtual Service Router (@hcaldicott)
- model for saos10 (@penfold1972)

### Changed
- remove uri in commit-archive location for EdgeOS. Fixed #3525 (@systeembeheerder)
- acos: remove free storage amount from show version. Fixes #3492 (@991jo)
- Housekeeping in the code: Maximal line length: 120 char + Rubocop fixes (@robertcheramy)
- spec/model/data uses # instead of : as a separator in the filename, so we can
  git clone under Windows. Fixes: #3481 (@robertcheramy)
- logging: rework of the logging system, using Semantic Logger (@robertcheramy)

### Fixed
- nxos: ignore bootflash size and permission errors (@rouven0)
- githubrepo: explicitly tell when Rugged isn't installed with ssh support (@robertcheramy)
- ironware: mask temperatures with more than two digits (@merelissdgr)
- add content-type header for PUT request in rest client (@deesel)
- docker: do not remove git. Fixes #3482 (@robertcheramy)
- awplus: fix skip password when enable=true (@shigechika)
- aosw: fix secret parsing (@rouven0)
- mlnxos: handle ANSI-ESC codes and pager requests. The prompt has been
  reengineered, open an issue if you experience timeouts. Fixes #3469 (@robertcheramy)
- Update installation instructions on Rocky Linux 9. Fixes #3368 (@robertcheramy)
- awplus: fix enable password when supplied (@sgsimpson)
- Fix CodeQL scanning alerts on regular expressions (Issue #3513) in node.rb (alert 40),
  asa (alerts 5 and 6), sonicos (4, 11), quantaos (9, 10), eltex (7), zynos (18, 19),
  AricentISS (15) and aosw (36)
- fabricos: remove power supply input voltage from `chassisShow` output (@hops)
- netgear: include running-config in config output (@bradleywehmeier)
- tmos: remove deprecated secrets (@rouven0)
- log an error when no suitable input is found for a node. Fixes: #3346 (@robertcheramy)
- firelinuxos: fix timeout on syntax error. Fixes #3393, #3502 (@robertcheramy)


## [0.33.0 - 2025-03-26]
This release changes the way to configure oxidized-web. The old `rest`
configuration is still supported but deprecated. The new configuration works
with oxidized-web 0.16.0 or later.
See [docs/Configuration.md](/docs/Configuration.md#oxidized-web-RESTful-API-and-web-interface).

The docker container includes
[security fixes to ruby-rake](https://ubuntu.com/security/notices/USN-7366-1),
so be sure to update to the latest version.

### Added
- unifiap: new model for Unifi APs, switches, and AirOS APs (@clifcox)
- github: Issue templates for bugs, feature requests and support requests (@robertcheramy)
- model for Ingate Operating System (@thanegill)
- model for Easton Gigabit Network Card (@thanegill)

### Changed
- fortios: support for FortiADC (@electrocret)
- output/git: cache commit log to improve performance of oxidized-web. Fixes #3121 (@robertcheramy)
- input/http: digest auth handles special characters in passwords by itself (no need to url encode them manually) (@einglasvollkakao)
- changed the configuration for oxidized-web from rest: to extensions.oxidized-web (@robertcheramy)
- netgear: add pager-handler workaround, fixes: #2394 and #3341 (@candlerb, @syn-bit)
- Output#version (git/gitcrypt) returns a Time object in its hash for more flexibility in oxidized-web (@robertcheramy)
- ios: hide secret key of aaa radius (@martinberg, @robertcheramy)
- container: update to phusion/baseimage:noble-1.0.1 (@robertcheramy)

### Fixed
- powerconnect: Mask the changing temperature issue for non-stacked switches. Fixes #2088 (@clifcox)
- Fix frozen string literals (@robertcheramy)
- powerconnect: Cleanup login/logout logic. Fixes #3437 (@clifcox)
- aos7: remove extra lines occuring when `show hardware-info` runs slow (@rouven0)
- srosmd: add ignore regex for 64-bit system uptime (@emiliaaah)
- removed some rubocop warnings (@robertcheramy)


## [0.32.2 – 2025-02-27]
This patch release mainly fixes the docker building process, wich resulted in
0.32.1 not beeing built.

### Changed
- docker image: updated github CI to explicitly build tag versions (@robertcheramy)
- docker image: update rake build_container to match the tags of GitHub CI (@robertcheramy)

### Fixed
- powerconnect: restore last line of command output, and remove spurious CR characters. Fixes #2692 (@clifcox)
- powerconnect: Remove undesirable inserted blank lines during pagination. Fixes #3413 (@clifcox)
- docker image: remove ubuntu user introduced in noble. Fixes #3336 (@robertcheramy)
- docker image: correct rights under /home/oxidized. Fixes #3336 (@robertcheramy)
- docker image: revert the use of GEM_HOME. Fixes #3331 (@robertcheramy)
- docker image: improve the documentation. Fixes #3336 (@robertcheramy)
- docker image: remove examples/podman-compose as this is better documented in docs/Docker.md (@robertcheramy)


## [0.32.1 – 2025-02-20]
This patch release fixes a javascript issue in oxidized-web, which is included
in the Docker container of oxidized.

### Fixed
- powerconnect: Hide enable, and line secrets. Further Fixes #1212 (#clifcox)


## [0.32.0 – 2025-02-17]
This release fixes a security issue in oxidized-web, which is included in the
Docker container of oxidized. If you are not using the Docker container but
use oxidized-web, be sure to update your oxidized-web gem to 0.15.0.

### Added
- junos: add unit test (@systeembeheerder)
- apc_aos: support for scp (@robertcheramy)
- config: allow model_map and group_map keys to be regexp. Fixes #3360 (@ytti)
- enterprise_sonic: add new model enterprise_sonic (@ohai89)
- model for Kornfeld Operating System (@yurenkov)

### Changed
- sonicos: accept policy message. Fixes #3339 (@Steve-M-C, @robertcheramy)
- input/ssh: change input.debug to dump all characters and include sent commands. (@robertcheramy)
- cumulus: remove ANSI Escape codes and fix prompt issues. The prompt is more specific now (@alchemyx, @robertcheramy)
- model unit tests: the tests are automated and simpler to use (@ytti, @robertcheramy)
- device2yaml.rb: moved to extra/, commands can be specified from the command line or from a file (no cmdsets provided anymore) (@robertcheramy)
- extra/gitdiff-msteams.sh: honor the 28KB size limit and add an optional link to GitHub (@mopi3456)

### Fixed
- tplink: send 'enable' before the enable password. Fixes #3271 (@robertcheramy)
- asyncos: fix prompt for hostnames containing "-" . Fixes #3327 (@robertcheramy)
- sonicos: fix prompt for hostnames containing "-" . Fixes #3333 (@robertcheramy)
- xos: Hide radius accounting secret
- fsos: Hide AAA and SNMP secrets (@RayaneB35)
- aos7: fix prompt for version 8.8x. Fixes #3351 (@robertcheramy)
- aosw: Hide power measurements (@rouven0)
- arubainstant: show version prepends a space to prompt when a core file is present. Fixes #3398 (@robertcheramy)


## [0.31.0 – 2024-11-29]

### Added
- model for Riverbed Steelhead (@Swaeltjie)
- model for uplink EP4440-DP OLT (@AAm-kun)
- model for Siklu Multihaul TG radios (@bdg-robert)
- model for VMWare NSX DFW (@elmobp)
- model for F5OS (@teunvink)
- cumulus: Add possibility to use NVUE (@lagertonne)
- model for garderos (@robertcheramy)
- unit tests framework for models with ssh input (@robertcheramy)
- os6: Added support to Dell EMC Networking OS6 (@anubisg1)
- model for HPE Aruba Networking Instant Mode (Aruba Instant). Fixes #3057 (@robertcheramy)
- Add JSONFILE source (@sargon)

### Changed
- h3c: change prompt to expect either angle (user-view) or square (system-view) brackets (@nl987)
- xos: Hide radius and user secrets for XOS (@iriseden)
- eos: Hide radius and snmp secrets for Arista EOS (@iriseden)
- fortios: Hide date in acme certifcate comments (@systeembeheerder)
- dlink: added support for 'enable admin' before getting configuration, if enable=true (@as8net)
- dlinknextgen: strip uptime and ntp update time from config
- Updated slackdiff.rb to use slack_ruby_client instead of slack-api (@Punicaa)
- oxidized: options (such as credentials, etc.) now use the same resolution logic as variables and can also be defined per model in a group (@EinGlasVollKakao)
- saos: add inventory and software status collection (@grbeneke)
- container-image: update to phusion/baseimage:noble-1.0.0 and include security upgrades at build time (@robertcheramy)
- container-image: use ubuntu-packages instead of gems in order to reduce container image size (@robertcheramy)
- edgecos.rb: hide temperature and fan speed (@dhooper6430)
- cnos: show information before config, remove secrets only when told to do so (@robje)
- Updated slackdiff.rb to use new files.getUploadURLExternal slack file upload API instead of deprecated files.upload (@varesa)
- Updated source/output files to reference a Source/Output module to avoid namespace duplication (@laf, @robertcheramy)
- ios: Hide WLAN PSK, AP profile dot1x password, AP profile mgmtuser password/secret and radius COA server-key (@devon-mar)
- ios: remove values from custom SNMP OID's, set by an EEM script (@syn-bit)
- Update net-ssh to 7.3 to enable support for aes(128|256)gcm. Fixes #3168 (@jacobw)
- removed time command from uplink EP4440-DP OLT model
- fortios: variable `fullconfig` to get the configuration with default values. Fixes: #3159 (@robertcheramy)
- container-image: install x25519 gem package to support more ssh kex. Fixes #3070 (@benasse)
- lenovonos: Salt administrator-password line when remove_unstable_lines is set to True (@kani999)
- lenovonos: Removes lines that started with Fan because RPM always changes. (@kani999)

### Fixed
- fixed error for ibos when remove_secret is set (@dminuoso)
- fixed prompt for Watchguard FirewareOS not matching the regex when the node is managed and master (@benasse)
- fixed prompt for vyos/vyatta to allow logins with non-priviliged accounts. Fixes #3111 (@h-lopez)
- fixed power consumption included in ArubaOS-CX diffs starting with FL.10.13.xxx. Fixes #3142 (@terratalpi)
- fixed oxidized-web getting "version not found" when fetching a version from git and no group is defined. Fixes #2222 (@robertcheramy)
- fixed telnet to disconnect gracefully even if it throws IOError while disconnect. Fixes #3212 (@ytti)
- docs: run git garbage collection to address performance issues. Fixes #3121 (@robertcheramy)
- saos: fixed handling of 'unsaved configuration' indicator in prompt (@grbeneke)
- rgos: also strip "System uptime" for installed modules (@spike77453)
- fixed digest authentication when using http input (@spike77453)
- fixed aosw prompt; now working with ArubaOS 8 (@mabezi, @robertcheramy)
- routeros: fix system info for CHR. Fixes #3180 (@systeembeheerder)
- removed hardcoded '~/.config/oxidized/config'. Fixes #3229 (@robertcheramy)
- linuxgeneric: updated prompt to support '$' at the end (@robertcheramy)
- hook githubrepo: add a specific warning when rugged not installed with ssh support. Fixes #3211 (@robertcheramy)
- hook githubrepo: works with custom branch names (@robertcheramy)
- ios: removes secrets when config has multiple snmp-server host lines. Fixes #3239 (@robertcheramy)
- ios: fixed device specs on ASR900 Series. Fixes #3297 (@robertcheramy, @roshnaraman)
- netgear: prompt for gs752tpp. Fixes #3287 (@robertcheramy)
- aoscx: fixed regex for 6400 switches to hide temperature and power (@steveneppler)


## [0.30.1 – 2024-04-12]

### Added
- document the release process (@robertcheramy)

### Fixed
- The version string was not set correctly (@robertcheramy)

## [0.30.0 – 2024-04-11]

### Added
- model for D-Link cisco like CLI (@mirackle-spb)
- model for Ruijie Networks RGOS devices (@spike77453)
- Add serial number collection to Opengear (@ermuller)
- added support for AricentISS 2.x firmware (@davromaniak)
- model for Asterfusion Network Operating System (@avl-dev)
- pagination for http source (@davama)
- model for Ericsson Miniling 6600 series (@schouwenburg)
- model for Mimosa B11 (@ritzbhuj)
- added SCP input (@aeiodelic)
- Added `linux/arm64` and `linux/amd64` platforms to Docker build/publish. (@disaac)
- Added verion info for Vyatta (@systeembeheerder)
- model for Fibrestore (fs.com) FSOS (@tcrichton)
- model for IP Infusion OcNOS
- model for Fujitsu 1finity (@jerji)

### Changed
- tp-link: fixed enable mode post login entrance (@mirackle-spb)
- dlink: fixed prompt for other dlink switches, added additional uptime removal expressions (@mirackle-spb)
- Collect VC info for juniper ex3400 (@ermuller)
- adva: fix config content for recovery process, collect config delta instead of current (@MichiMeyer)
- iosxr: include last config changed by in model (@electrocret)
- panos: exclude device dictionary
- Added support for Nokia SAR 7705 HMC in SROS model (@schouwenburg)
- fortios: only perform a "show" instead of a "show full-configuration" when retrieving configs from fortios. fixes timeouts and avoids extraneous defaulted config information. (@jforeman)
- opengear: support newer Opengear CM* and OM* models (@matej_v)
- edgecos: improve system temperature removal (@freddy36)
- zynos: Rewrite the script to properly collect config via ssh/telnet. Backup with FTP is not working atm, feel free to open an issue if needed. (@sharteeya)
- pfsense: exclude autogenerated firewall rule timestamps to reduce change churn from use of stuff like PFBlockerNG #2985 (@anthonysomerset)
- ciscosmb: ignore "Please change the password" hint when doing backup. (@sharteeya)
- c4cmts: uses chassis eeprom data for inventory information instead of environmental status (@nickhilliard)
- dlinknextgen removes user and snmp-server secrets (@tcrichton)
- dnos: hide secrets in "ospf message-digest-key", "authentication-type" and "bsd-username" (@rybnico)
- junos: Replace dynamic value in VMX-BANDWIDTH with count, hide ssh keys

### Fixed
- fixed empty lines for ZyXEL GS1900 switches (@jluebbe)
- fixed prompt for Watchguard FirewareOS not matching the regex when the node is non-master (@netdiver)
- defined 'psych' runtime dependency to resolve 'unsafe_load' error during startup (@MattKobayashi)
- fixed new date/time format with newer RouterOS `# jun/01/2023 12:11:25 by RouterOS 7.9.1` vs `# 2023-06-01 12:16:16 by RouterOS 7.10rc1` (@tim427)
- fixed netscaler backups with hostname set #2828 (@electrocret)
- Do not redirect stderr when fetching opnsense version since default shell (csh) doesn't support it (@spike77453)
- Fix missing configuration for Opengear (@ermuller)
- Fixed fan RPM speeds included in Aruba CX diffs (@danpoltawski)
- Gitcrypt output refinements (@electrocret)
- Remove constantly updating dates from backup of Adtran config (@davesbell)
- fixed prompt for Cumulus to allow usernames with dots and dashes (@ktims)
- fixed source http when source is librenms (@davama)
- Fixed login and su regex unable to match in some comware devices (@sharteeya)
- fixed prompt detection for Netgear M4250-10G2XF-PoE+ and M4300-28G-PoE+ (@rexhaugen)
- fixed devices (pfsense, opnsense, openwrt) not retriving config after refinement change #2771 #2968 (@robertcheramy)
- Fixed login into Fortigate when post-login-baned ist enabled. Fixes #2021 (@chrisr0880, @sahdan, @dangoscomb and @robertcheramy)
- Fixed pre_logout for BDCOM switches
- Fix 'wpa passphrase' hashed secret for SonicOS devices with built-in wireless #3036 (@lazynooblet)
- Oxidized will now exit on SIGTERM and SIGINT; SIGHUP will trigger a node list reload and reopen logs (@gs-kamnas)
- Fix potential busy wait when retries and/or next_adds_job is enabled (@gs-kamnas)
- Reverting PR #2498 as it broke old procurve models (2510G, 2610, 2824). Fixes #2833, #2871 (@robertcheramy)
- Fixed regexp used to remove secrets in nxos model. Fixes #3080 (@desnoe)

## [0.29.1 - 2023-04-24]

### Added

### Changed

- Ensure Docker builds are only pushed to registry on `master` (@aschaber1)

### Fixed

- #2749: fix issues with `cut_both`

## [0.29.0 - 2023-04-13]

- Ensure Docker Builds are tagged with latest and version (using `git describe --tags`) (@aschaber1)
- Upgrade Dockerfile baseimage to Ubuntu 22.04 and Ruby 3.0 (@aschaber1)
- Upgrade Ruby dependency to Ruby 3.0+ (@aschaber1)
- model for PanOS & Panorama via HTTP API (@pv2b, @sts)
- model for MikroTik SwOS devicse (@sm-nessus)
- model for TrueNAS devices (@neilschelly)
- model for Acme Packet devices (@ha36d)
- model for SmartCS devise (@yoshihito-tatano)
- Cumulus: added option to use NCLU as ia collecting method
- Update net-ssh to 7.0.1 (using `append_all_supported_algorithms: true`)
- Allow (config) (vlan-####) confirmation (y/n) and sftp questions in procurve prompt (@sorano)
- Added new model called `srosmd` to backup configuration fron Nokia SR OS devices running in the model-driven CLI mode, which has different syntax to existing `sros` model.
- fix: allow Netgear devices to finish SSH session correctly
- More pager handling for MIS5030Q (@glance-)
- Update logrotate example to allow logrotate service to start before any logs exist
- Mask NX-OS tacacs+ host keys (@0x4c6565)
- airfiber: prompt matching made case-insensitive to support AF5xHD (@noaheroufus)
- Cumulus: add support for dhcp-relay (@ohai89)
- Github Action: Support creating custom version tag in a release
- Add support for no enable password set on ironware
- change pfSense secret scrubbing to keep config as well-formed XML
- Add support for comware HPE Office Connect 1950
- Added model for Hirschmann Hios devices, next to alread present Hirschmann classic (@tijldeneut)
- (StoneOS) Prompt fix for long device names, '^H' handling and removal of ever-changing data (@dMailonG)

### Added

- install rugged with ssh in Dockerfile (@agrevtcev)
- allow to run as non-root user in Docker (@agrevtcev)
- Extend http source configurations to include read_timeout value
- model for bdcom-based fiberstore switches (@candlerb)
- enterasys800 model for enterasys 800-series fe/ge switches (@javichumellamo)
- add ES3526XA-V2 support in EdgeCOS model (@moisseev)
- model for eltex mes-series switches (@glaubway)
- model for zte c300 and c320 olt (@glaubway)
- model for LANCOM (@systeembeheerder)
- model for Aruba CX switches (@jmurphy5)
- model for FortiWLC WLAn Controller (@MrMarioMichel)
- model for NEC IX devices (@mikenowak)
- Added docs for Dell/EMC Networking OS10 devices (@davromaniak)
- model for Zyxel 1308 OLTs (@baldoarturo)
- model for Linksys SRW switches (@glance-)
- Added exec hook variables to retrieve verbose node's failure reason and type
- model for Cambium ePMP radios (@martydingo)
- Added GaiaOS support for virtual systems (@rwxd)
- Added new `group_map` configuration option (@mjbnz)
- Dockerfile rebased to phusion/baseimage-docker focal-1.2.0
- model for Lenovo Network OS (@seros1521)
- new option: use_max_threads
- model for ADVA devices (@stephrdev)
- model for YAMAHA NVR/RTX Series (@bluekirin55)
- model for ZPE Nodegrid OS (@euph333)
- model for H3C switches
- model for Cisco Catalyst Express switches (@unemongod)
- extended mysql source configuration to include tls options (@glaubway)
- updated rugged in gemspec for ruby 3.0 support (@firefishy)
- Added exec hook for MS Teams webhook (@systeembeheerder)

### Changed

- Better manage of the enable mode in edgeswitch.rb (@agabellini)
- Adds paging support to Enterasys B3/C3 (@piterpunk)
- Allows "Username" as username prompt in Brocade ICX-series devices (@piterpunk)
- Add show-sensitive flag on export command on Mikrotik RouterOS when remove_secret is off (@kedare)
- rubocop dependency now ~> 0.81.0, the last one with ruby 2.3 support
- change pfSense secret scrubbing to handle new format in 2.4.5+
- Dockerfile rebased to phusion/baseimage-docker bionic-1.0.0
- scrub PoE related messages from routeros config output (@pioto)
- support for d-link dgs-1100 series switches in dlink model (@glaubway)
- enterasys model now works with both ro and rw access (@sargon)
- restore including last configuration change IOS if the change is done by real user. Resolves #1921 (@raunz)
- Additional scrubbing for SonicOS v7 devices (@gerard780)
- improved Telnet support for enterasys (@jplitza)
- Include "show version" output for enterasys (@jplitza)
- xmppdiff now also shows diffs with only removed or only added lines (@jplitza)
- xmppdiff now persists its connection to the XMPP server and MUC (@jplitza)
- routeros no longer backups infos on available updates (@jplitza)
- avoid /humidity hardware field in tmos (F5) to be reported (@albsga)
- read all configurations in partition folder for tmos (F5) (@dalamanster)
- version information or OPNsense and PFsense models is now included as XML comments (@pv2b)
- netscaler, backup all partitions (@smallsam)
- only runs SSH proxy commands if the ssh_proxy configuration item has been defined (@jameskirsop)
- updated vrp.rb to correctly parse huawei devices
- asa: information about the configuration change time is deleted
- Extended groups configuration to support models vars within a group (@mjbnz)
- Extended `remote_repo` configuration to allow repo specific ssh keys (@mjbnz)
- sonicos: added scrubbing for hashed values (@televat0rs)
- nxos: Additional scrubbing for nxos device passwords (@derekivey)
- nxos: Fix password match to avoid stripping out the user role. (@derekivey)
- OpenBSD: Include bgpd, ospfd and ospf6d files (@woopstar)
- scrub often changing values in junos license output (@matejv)
- comware: support for enable(super) login password added (@delvta)
- use slack-ruby-client instead of slack-api for slackdiff hook (@0xmc)
- ios: Add support for RBAC in IOS model (@jameskirsop)
- hide unsupported-transceiver license key in Arista EOS (@davidc)
- edgecos: add support for FS S3900-48T6S-R (@cgsecurity)
- eltex: added one command for disable pagination on Eltex MES2424 switches (@mirackle-spb)

### Fixed

- fixed an issue for IOSXR devices which can't allocate pty because of problem with forwarding agent.
- fixed an issue with the TrueNAS model where the configuration would come up blank when TrueNAS was also using it. (@neilschelly)
- fixed on issue where Oxidized could not pull config from Opengear devices #1899 (@rikard0)
- fixed an issue where Oxidized could not pull config from XOS-devices operating in stacked mode (@DarkCatapulter)
- fixed an issue where Oxidized could not pull config from XOS-devices that have not saved their configuration (@DarkCatapulter)
- improved scrubbing of show chassis in ironware model (@michaelpsomiadis)
- fixed snmp secret handling in netgear model (@CirnoT)
- generalise fortiOS to support new versions and VM based products e.g. FortiManager, FortiAnalyzer and FortiMail, which requires keyboard-interactive auth method. Includes trial-and-error cmd list to retrieve most infromative config. Fixes #2227. Moved 'system ha status' to 'config global' section to support clusters with virtual domains.
- filter next periodic save schedule time in xos model output (@sargon)
- Fix when auto-saved is configured on xos switches  (@trappiz)
- fixed ArubaOS-CX enviroment/system inconsistent values #2297 (@raunz)
- further improvements to ArubaOS-CX environment values (@olemyhre)
- Update AirFiber prompt regex (@murrant)
- System time and running time are now stripped from tplink model output (@spike77453)
- &lt;?xml... line is no longer improperly stripped from OPNsense and PFsense backups (@pv2b)
- fixed an issue where Oxidized timeouts in Brocade ICX-series devices (@piterpunk)
- fixed an issue where EOS config was truncated. Fixes #2038 (@jake2184 @fhibler)
- fixed missing output from routeros version command (@mjbnz)
- stopped `clear: true` from removing all commands (@mjbnz)
- Updated fastiron enable password prompt regex (@pepperoni-pi)
- fixed an issue where the pfsense model would not report errors in case it was unable to download the configuration e.g. due to insufficient permissions
- added a missing check for whether to send `enable` commands to Adtran devices (@repnop)
- fixed an issue where ADVA devices with a configured prompt != `ADVA-->` couldn't be polled (@momorientes)
- ensure local time and system up time are filtered for ADVA devices (@stephrdev)
- fixed an issue with FortiOS that didn't accurately match `set ca` lines #2567 (@neilschelly)
- removed unwanted current date from slxos model
- fixed secret handling for rip authentication in casa model #2648 (@grahamjohnston)
- stripped IoT-detect version for Fortigate devices
- Fix expect usage in models on telnet. Tested with Netgear GS108T. (@arrjay)
- Fix Sonicwall SonicOS "system-uptime" omission from log (@lazynooblet)
- purityos: at least v6.3.5 needs other terminal settings (@elliot64)
- purityos: remove purealerts and VEEAM snapshots from backed up config (@elliot64)

## [0.28.0 - 2020-05-18]

### Added

- add VMWare NSX Edge 6.4+ support (@elmobp)
- add Dell EMC Networking OS10 support (@mmisslin)
- add Centec Networks CNOS (Fiberstore S5800/S5850) support via cnos model (@freddy36)
- include transceiver information in EdgeCOS model (@freddy36)
- add Telco Systems T-Marc 3306 support via telco model (@SkylerBlumer)
- add enable support to ciscosmb (@deesel)
- add Waystream iBOS model
- add QTECH model (@moisseev)

### Changed

- more secret scrubbing in sonicos (@s-fu)
- openssh key scrubbing as secret in fortios (@agabellini)
- scrubs macsec key from Arista EOS (@krisamundson)
- rubocop dependency now ~> 0.80.0
- rugged dependency now ~> 0.28.0
- cumulus model no longer records transient data (@plett)

### Fixed

- use without-paging variant of print statement in routeros (@vkushnir)
- include the commands in the output in EdgeCOS model (@freddy36)
- update patterns for minor software version dependent differences in EdgeCOS model @freddy36)
- better login modalities for telnet in aos7 (@optimuscream)
- better virtual domain detection in fortios (@agabellini)
- allow any max length for username/password in GcomBNPS (@freddy36)
- relax prompt requirements in ciscosmb (@Atroskelis)
- fortios model strips uptime even without remove_secrets (@jplitza)
- HP ProCurve now accepts ">" as apart of the prompt (@magnuslarsen)
- fix IOS SNMP notification community hiding for informs and v3 (@moisseev)
- fixed issue where the regex-pattern for XOS-prompts used invalid syntax (@DarkCatapulter)
- set terminal width in EdgeCOS model (@moisseev)
- suppress errors for commands that are not supported on some devices in EdgeCOS model (@moisseev)
- revert including command names in the output of the EdgeCOS model (@moisseev)

## [0.27.0] - 2019-10-27

### Added

- add automatic restart on failure for systemd (@deajan)
- add ZynOS GS1900 specific model support (@deajan)
- add PurityOS model support (@elliot64)
- add Ubiquiti Airfiber model support (@cchance27)
- add Icotera support (@funzoneq)
- include licensing information in aos model (@pozar)
- include chassis information in sros model (@raunz)
- add firelinuxos (FirePOWER) model (@rgnv)
- add sonicos model (@rgnv)
- add hpmsm model (@timwsuqld)
- include hardware and product information in oneos model (@raunz)
- add FastIron model (@ZacharyPuls)
- add Linuxgeneric model (@davama)
- include HA status info in fortios model (@raunz)
- add SpeedTouch model (@raunz)
- comware added device manuinfo to include serial number (@raunz)

### Changed

- add pgsql support, mechanized and net-tftp to Dockerfile
- upgrade slop, net-telnet and rugged
- extra secret scrubbing in comware model (@bengels00)
- removed snmpd lines from linuxgeneric model
- moved show configuration command to the end in junos model (@raunz)
- filter pap and chap passwords in ios model (@matejv)

### Fixed

- prevent versionning on procurve switches by removing power usage output (@deajan)
- improve procurve telnet support for older switches (@deajan)
- voss model
- cambium model should not consider timestamp for backup as unneeded, and causes diffs -@cchance27)
- remove 'sh system' from ciscosmb model (@Exordian)
- dlink model didn't support prompts with spaces in the model type (Extreme EAS -00-24p) (@cchance27)
- routeros model does not collect configuration via telnet input (@hexdump0x0200)
- add dependencies for net-ssh
- don't log power module info on procurve model anymore
- crash on some recent Ruby versions in the nagios check (@Kegeruneku)
- remove stray whitespace in adtran model (@nickhilliard)
- if input model returns subclassed string we may overwrite the string with an empty -tring
- updated aosw.rb prompt. addresses issue #1254
- update comware model to fix telnet login/password for HPE MSR954 and HPE5130. Issue -1886
- filter out IOS configuration/NVRAM modified/changed timestamps to keep output -ersistent
- update screenos model to reduce the amount of lines being stripped from beginning of -fg output
- include colon in aosw prompt regexp in case it is a mac address (@raunz)
- comware improvement for requesting HP 19x0 switches hidden CLI. Issues #1754 and -1447
- fix variable inheritance when subclassing a model

## [0.26.3] - 2019-03-06

### Fixed

- regression in git.rb version method where we check if Rugged::Diff has any deltas/patches

## [0.26.2] - 2019-03-05

### Fixed

- suppress net-ssh 5 deprecation warnings by moving from :paranoid to :verify_host_key

## [0.26.1] - 2019-03-04

### Fixed

- force file permissions in rubygems

## [0.26.0] - 2019-03-04

### Added

- add Cisco VPN3000 model (@baznikin)
- add NetGear PROSafe Smart switches model (@baznikin)
- Added possibility to pass root logs directory as environment variable (@Glorf)
- add OneAccess TDRE (1645) model (@starrsl)
- add Audiocodes MediaPack MP-1xx and Mediant 1000 model (@pedjaj)
- add raisecom RAX model (@vitalisator)
- add huawei smartax model (@nyash)
- add grandstream model

### Changed

- prompt updates in siklu, netonix, netscaler models
- minimal supported ruby is now 2.3, net-ssh dependency ~> 5, rubocop ~> 0.65.0

### Fixed

- in git comparison we might mistakenly always detect change due to !utf8 vs. utf8 encoding of a char

## [0.25.1] - 2018-12-18

### Fixed

- update changelog which was forgotten during release

## [0.25.0] - 2018-12-16

### Added

- add viptela model (@bobthebutcher)
- add ECI Telecom Appolo platform bij arien.vijn@linklight.nl
- ssh keepalive now configurable per node with ssh_no_keepalive boolean
- add Comtrol model (@RobbFromIT)
- add Dell X-series model (@RobbFromIT)
- add privilege escalation to the cumulus model (@user4574)
- add adtran model (@CFUJoshWeepie)
- add firebrick model (@lewisvive)

### Changed

- bump Dockerfile phusion/baseimage:0.10.0 -> 0.11, revert to one-stage build
- add sqlite3 and mysql2 drivers for sequel to Dockerfile
- Added verbiage to set OXIDIZED_HOME correctly under Debian 8.8 w/systemd
- add gpgme and sequel gems to Dockerfile for sources
- eos model removes user secrets and BGP secrets (@yzguy)
- add secret filtering to netscaler (@shepherdjay)
- capture ZebOS configuration for TMOS model (@yzguy)
- additional secret filters in ios, asa, procurve, ciscosmb models (@hexdump0x0200)
- remove volatile uptime data in nos model (@f0rkz)

### Fixed

- netgear telnet password prompt not detected
- xos model should not modify config on legacy Extreme Networks devices (@sq9mev)
- model dlink, edgecos, ciscosmb, openbsd
- hide 'lighttpd_ls_password' as potential secret in pfsense model (@dra)
- ciscospark hook error when diff is set to false

## [0.24.0] - 2018-06-14

### Added

- add frr support to cumulus model (@User4574 / @bobthebutcher)
- honour MAX_STAT in mtime, to store last N mtime
- configurable stats history size
- model callback enhancements for customizing existing models (@ytti)

### Fixed

- models ciscosmb, dlink

## [0.23.0] - 2018-06-11

### Added

- support arbitrary user/password/prompt detection in telnet, same behaviour as ssh
- manager refactor, support local loading of input, output, source, not just model and hook
- store modification time in node stats

### Fixed

- model edgecos does not trigger false positives due to uptime and memory utilization (@sq9mev)
- Use SECRET-DATA hints for hiding secrets in JunOS (@Zmegolaz)
- comware (@adamboutcher)

## [0.22.0] - 2018-06-03

### Added

- openbsd model (@amarti2038)
- comnet model (@jaylik)
- stoneos model (@macaty)
- openwrt model (@z00nx)
- arbos model (@jsynack)
- ndms model (@yuri-zubov)
- openwert model (@z00nx)
- stoneos model (@macaty)
- comnetms model (@jaylik)
- openbsd model (@amarti2038)
- cambium model
- ssh key passphrase (@wk)
- cisco spark hook (@rgnv)
- added support for setting ssh auth methods (@laf)

### Fixed

- models procurve, br6910, vyos, fortios, edgeos, vyatta, junos, powerconnect, supermicro, fortios, firewareos, aricentiss, dnos, nxos, hpbladesystem, netgear, xos, boss, opengear, pfsense, asyncos

## [0.21.0] - 2017-11-01

### Added

- routeros include system history (@InsaneSplash)
- vrp added support for removing secrets (@bheum)
- hirschmann model (@OCangrand)
- asa added multiple context support (@marnovdm)
- procurve added additional output (@davama)
- Updated git commits to bare repo + drop need for temp dir during clone (@asenci)
- asyncos model (@cd67-usrt)
- ciscosma model (@cd67-usrt)
- procurve added transceiver info (@davama)
- routeros added remove_secret option (@spinza)
- Updated net-ssh version (@Fauli83)
- audiocodes model (@Fauli83)
- Added docs for Huawei VRP devices (@tuxis-ie)
- ciscosmb added radius key detection (@davama)
- radware model (@sfini)
- enterasys model (@koenvdheuvel)
- weos model (@ignaqui)
- hpemsa model (@aschaber1)
- Added nodes_done hook (@danilopopeye)
- ucs model (@WiXZlo)
- acsw model (@sfini)
- aen model (@ZacharyPuls)
- coriantgroove model (@nickhilliard)
- sgos model (@seekerOK)
- powerconnect support password removal (@tobbez)
- Added haproxy example for Ubuntu (@denvera)

### Fixed

- fiberdriver remove configuration generated on from diff (@emjemj)
- Fix email pass through (@ZacharyPuls)
- iosxr suppress timestamp (@ja-frog)
- ios allow lowercase user/pass prompt (@deepseth)
- Use git show instead of git diff (@asenci)
- netgear fixed sending enable password and exit/quit (@candlerb)
- ironware removed space requirement from password prompt (@crami)
- dlink removed uptime from diff (@rfdrake)
- planet removed temp from diff (@flokli)
- ironware removed fan, temp and flash from diff (@Punicaa)
- panos changed exit to quit (@goebelmeier)
- fortios remove FDS address from diffs (@bheum)
- fortios remove additional secrets from diffs (@brunobritocarvalho)
- fortios remove IPS URL DB (@brunobritocarvalho)
- voss remove temperature, power and uptime from diff (@ospfbgp)

## [0.20.0] - 2017-05-14

### Added

- gpg support for CSV source (@elmobp)
- slackdiff (@natm)
- gitcrypt output model (@clement-parisot)
- model specific credentials (@davromaniak)
- hierarchical json in http source model
- next-adds-job config toggle (to add new job when ever /next is called)
- netgear model (@aschaber1)
- zhone model (@rfdrake)
- tplink model (@mediumo)
- oneos model (@crami)
- cisco NGA model (@udhos)
- voltaire model (@clement-parisot)
- siklu model (@bdg-robert)
- voss model (@ospfbgp)

### Fixed

- ios, cumulus, ironware, nxos, fiberdiver, aosw, fortios, comware, procurve, opengear, timos, routeros, junos, asa, aireos, mlnxos, pfsense, saos, powerconnect, firewareos, quantaos

## [0.19.0] - 2016-12-12

### Added

- allow setting ssh_keys (not relying on openssh config) (@denvera)
- fujitsupy model (@stokbaek)
- fiberdriver model (@emjemj)
- hpbladesystems model (@flokli)
- planetsgs model (@flokli)
- trango model (@rfdrake)
- casa model (@rfdrake)
- dlink model (@rfdrake)
- hatteras model (@rfdrake)
- ability to ignore SSL certs in http (@laf)
- awsns hooks, publish messages to AWS SNS topics (@natm)

### Fixed

- pfsense, dnos, powerconnect, ciscosmb, eos, aosw

## [0.18.0] - 2016-10-14

### Added

- APC model (by @davromaniak)

### Fixed

- ironware, aosw
- interpolate nil, false, true for node vars too

## [0.17.0] - 2016-09-28

### Added

- "nil", "false" and "true" in source (e.g. router.db) are interpeted as nil, false, true. Empty is now always considered empty string, instead of in some cases nil and some cases empty string.
- support tftp as input model (@MajesticFalcon)
- add alvarion model (@MajesticFalcon)
- detect if ssh wants password terminal/CLI prompt or not
- node (group, model, username, password) resolution refactoring, supports wider range of use-cases

### Fixed

- fetch for file output (@danilopopeye)
- net-ssh version specification
- routeros, catos, pfsense

## [0.16.3] - 2016-08-25

### Added

- pfsense support (by @stokbaek)

### Fixed

- cumulus prompt not working with default switch configs (by @nertwork)
- disconnect ssh when prompt wasn't found (by @andir)
- saos, asa, acos, timos updates, cumulus

## [0.16.2] - 2016-07-28

### Fixed

- when not using git (by @danilopopeye)
- screenos update

## [0.16.1] - 2016-07-22

### Fixed

- unnecessary puts statement removed from git.rb

## [0.16.0] - 2016-07-22

### Added

- support Gaia OS devices (by @totosh)

### Fixed

- #fetch, #version fixes in nodes.rb (by @danilopopeye)
- procurve

## [0.15.0] - 2016-07-11

### Added

- disable periodic collection, only on demand (by Adam Winberg)
- allow disabling ssh exec mode always (mainly for oxidized-script) (by @nickhilliard)
- support mellanox devices (by @ham5ter)
- support firewireos devices (by @alexandre-io)
- support quanta devices (by @f0o)
- support tellabs coriant8800, coriant8600 (by @udhos)
- support brocade6910 (by @cardboardpig)

### Fixed

- debugging, tests (by @ElvinEfendi)
- nos, panos, acos, procurve, eos, edgeswitch, aosw, fortios updates

## [0.14.3] - 2016-05-25

### Fixed

- fix git when using multiple groups without single_repo

## [0.14.2] - 2016-05-19

### Fixed

- git expand path for all groups
- git get_version, teletubbies do it again
- comware, acos, procurve models

## [0.14.1] - 2016-05-06

### Fixed

- git get_version when groups and single_repo are used

## [0.14.0] - 2016-05-05

### Added

- support supermicro swithes (by @funzoneq)
- support catos switches

### Fixed

- git+groups+singlerepo (by @PANZERBARON)
- asa, tmos, ironware, ios-xr
- mandate net-ssh 3.0.x, don't accept 3.1 (numerous issues)

## [0.13.1] - 2016-03-30

### Fixed

- file permissions (Sigh...)

## [0.13.0] - 2016-03-30

### Added

- http post for configs (by @jgroom33)
- support ericsson redbacks (by @roedie)
- support motorola wireless controllers (by @roadie)
- support citrix netscaler (by @roadie)
- support datacom devices (by @danilopopeye)
- support netonix devices
- support specifying ssh cipher and kex (by @roadie)
- rename proxy to ssh_proxy (by @roadie)
- support ssh keys on ssh_proxy (by @awix)

### Fixed

- various (by @danilopopeye)
- Node#repo with groups (by @danilopopeye)
- githubrepohoook (by @danilopopeye)
- fortios, airos, junos, xos, edgeswitch, nos, tmos, procurve, ipos models

## [0.12.2] - 2016-02-06

### Fixed

- more MRV model fixes (by @natm)

## [0.12.1] - 2016-02-06

### Fixed

- set term to vty100
- MRV model fixes (by @natm)

## [0.12.0] - 2016-02-05

### Added

- enhance AOSW (by @mikebryant)
- F5 TMOS support (by @mikebryant)
- Opengear support (by @mikebryant)
- EdgeSwitch support (by @doogieconsulting)

### Fixed

- rename input debug log files
- powerconnect model fixes (by @Madpilot0)
- fortigate model fixes (by @ElvinEfendi)
- various (by @mikebryant)
- write SSH debug to file without buffering
- fix IOS XR prompt handling

## [0.11.0] - 2016-01-27

### Added

- ssh proxycommand (by @ElvinEfendi)
- basic auth in HTTP source (by @laf)

### Fixed

- do not inject string to output before model gets it
- store pidfile in oxidized root

## [0.10.0] - 2016-01-06

### Added

- Various refactoring (by @ElvinEfendi)
- Ciena SOAS support (by @jgroom33)
- support group variables (by @supertylerc)

### Fixed

- various ((orly))  (by @marnovdm, @danbaugher, @MrRJ45, @asynet, @nickhilliard)

## [0.9.0] - 2015-11-06

### Added

- input log now uses devices name as file, instead of string from config (by @skoef)
- Dell Networkign OS (dnos) support (by @erefre)

### Fixed

- CiscoSMB, powerconnect, comware, xos, ironware, nos fixes

## [0.8.1] - 2015-09-17

### Fixed

- restore ruby 1.9.3 compatibility

## [0.8.0] - 2015-09-14

### Added

- hooks (by @aakso)
- MRV MasterOS support (by @kwibbly)
- EdgeOS support (by @laf)
- FTP input and Zyxel ZynOS support (by @ytti)
- version and diffs API For oxidized-web (by @FlorianDoublet)

### Fixed

- aosw, ironware, routeros, xos models
- crash with 0 nodes
- ssh auth fail without keyboard-interactive

## [0.7.1] - 2015-05-26

### Fixed

- rugged is again in gemspec (mandatory) (@ytti)

## [0.7.0] - 2015-05-21

### Added

- support http source (by @laf)
- support Palo Alto PANOS (by @rixxxx)

### Fixed

- screenos fixes (by @rixxxx)
- allow 'none' auth in ssh (spotted by @SaldoorMike, needed by ciscosmb+aireos)

## [0.6.0] - 2015-04-22

### Added

- support cumulus linux (by @FlorianDoublet)
- support HP Comware SMB siwtches (by @sid3windr)
- remove secret additions (by @rodecker)
- option to put all groups in single repo (by @ytti)
- expand path in source: csv: (so that ~/foo/bar works) (by @ytti)

### Fixed

- screenos fixes (by @rixxxx)
- ironware fixes (by @FlorianDoublet)
- powerconnect fixes (by @sid3windr)
- don't ask interactive password in new net/ssh (by @ytti)

## [0.5.0] - 2015-04-03

### Added

- Mikrotik RouterOS model (by @emjemj)
- add support for Cisco VSS (by @MrRJ45)

### Fixed

- general fixes to powerconnect model (by @MrRJ45)
- fix initial commit issues with rugged (by @MrRJ45)
- pager error for old dell powerconnect switches (by @emjemj)
- logout error for old dell powerconnect switches (by @emjemj)

## [0.4.1] - 2015-03-10

### Fixed

- handle missing output file (by @brandt)
- fix passwordless enable on Arista EOS model (by @brandt)

## [0.4.0] - 2015-03-06

### Added

- allow setting IP address in addition to name in source (SQL/CSV)
- approximate how long it takes to get node from larger view than 1
- unconditionally start new job if too long has passed since previous start
- add enable to Arista EOS model
- add rugged dependency in gemspec
- log prompt detection failures

### Fixed

- xos while using telnet (by @fhibler)
- ironware logout on some models (by @fhibler)
- allow node to be removed while it is being collected
- if model returns non string value, return empty string
- better prompt for Arista EOS model (by @rodecker)
- improved configuration handling for Arista EOS model (by @rodecker)

## [0.3.0] - 2015-02-13

### Added

- *FIXME* bunch of stuff I did for richih, docs needed
- ComWare model (by erJasp)
- Add comment support for router.db file
- Add input debugging and related configuration options

### Fixed

- Fix ASA model prompt
- Fix Aruba model display
- Fix changing output in PowerConnect model

## [0.2.4] - 2015-02-02

### Added

- Cisco SMB (Nikola series VxWorks) model by @thetamind
- Extreme Networks XOS model (access by sjm)
- Brocade NOS (Network Operating System) (access by sjm)

### Fixed

- Match exactly to node[:name] if node[name] is an ip address.

## [0.2.3] - 2014-08-16

### Added

- Alcatel-Lucent ISAM 7302/7330 model added by @jalmargyyk
- Huawei VRP model added by @jalmargyyk
- Ubiquiti AirOS added by @willglyn
- Support 'input' debug in config, ssh/telnet use it to write session log

### Fixed

- rescue @ssh.close when far end closes disgracefully (ALU ISAM)
- bugfixes to models

## [0.2.2] - 2014-07-24

### Fixed

- mark node as failure if unknown error is raised

## [0.2.1] - 2014-07-24

### Fixed

- vars variable resolving for main level vars

## [0.2.0] - 2014-07-24

### Added

- Force10 model added by @lysiszegerman
- ScreenOS model added by @lysiszegerman
- FabricOS model added by @thakala
- ASA model added by @thakala
- Vyattamodel added by @thakala

### Fixed

- Oxidized::String convenience methods for models fixed

## [0.1.1] - 2014-07-21

### Fixed

- vars needs to return value of r, not value of evaluation
