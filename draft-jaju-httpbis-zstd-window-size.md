---
title: "Window Sizing for Zstandard Content Encoding"
category: info

docname: draft-jaju-httpbis-zstd-window-size-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: WIT
# workgroup: HTTPBIS
keyword:
 - zstd
 - zstandard
 - content encoding
 - application/zstd
venue:
  # group: WG
  # type: Working Group
  # mail: WG@example.com
  # arch: https://example.com/WG
  github: nidhijaju/draft-zstd-window-size
  latest: https://nidhijaju.github.io/draft-zstd-window-size/LATEST

author:
 -
    fullname: Nidhi Jaju
    organization: Google
    email: nidhijaju@google.com

normative:

informative:


--- abstract

- deployments of zstd can use different
- larger causes more memory to be used
- browsers and user agents limit the max window size allowed
- this can cause incompitabilities if the the compression content uses a larger
limit
- this document updates rfc8878 to standardize a window size limit associated
with the token "zstd" and discusses considerations for negotiated out of band


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Window Sizes

# "zstd" Content Encoding Token
- update the actual text in rfc8878
- change recommendation to MUST

# Negotiating out-of-band

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.

## application/zstd (?)

## Content Encoding


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
