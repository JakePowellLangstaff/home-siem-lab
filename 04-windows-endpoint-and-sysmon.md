<Sysmon schemaversion="4.90">
  <HashAlgorithms>SHA256</HashAlgorithms>
  <CheckRevocation />

  <EventFiltering>
    <!-- With no matching exclusions, log all process creation events. -->
    <ProcessCreate onmatch="exclude" />

    <!-- With no matching exclusions, log all network connection events. -->
    <NetworkConnect onmatch="exclude" />
  </EventFiltering>
</Sysmon>

