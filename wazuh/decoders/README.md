# Wazuh GoPhish Decoders

## Purpose

SIM-001 uses custom Wazuh decoding to distinguish GoPhish application logs from unrelated endpoint telemetry and expose useful fields to downstream rules.

## Observed Decoder Configuration

The configuration retrieved from the running Wazuh manager was:

```xml
<decoder name="gophish">
  <prematch>msg=</prematch>
</decoder>

<decoder name="gophish-detail">
  <parent>gophish</parent>
  <regex>level=(\S+) msg="([^"]+)"</regex>
  <order>gophish_level, gophish_msg</order>
```

The displayed lab file ended after the `<order>` line. This documentation records the deployed configuration as observed rather than silently changing it.

## Parent Decoder: `gophish`

The parent decoder uses:

```xml
<prematch>msg=</prematch>
```

to identify candidate GoPhish-formatted events.

## Child Decoder: `gophish-detail`

The child decoder inherits from `gophish` and applies:

```xml
<regex>level=(\S+) msg="([^"]+)"</regex>
```

The capture groups are assigned in this order:

1. `gophish_level`
2. `gophish_msg`

## Detection Relationship

```text
GoPhish log entry
      |
      v
gophish parent decoder
      |
      v
gophish-detail child decoder
      |
      +--> gophish_level
      +--> gophish_msg
      |
      v
Rule 100010
```

The decoder layer performs parsing/classification. Alert severity and phishing-specific logic are implemented in the rules layer.

## Validation

The Wazuh event generated during SIM-001 reported `decoder.name = gophish`, confirming that the event passed through the custom GoPhish decoding path.
