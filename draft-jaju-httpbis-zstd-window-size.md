---
title: "Window Sizing for Zstandard Content Encoding"
abbrev: "Zstd Window Size"
category: info

docname: draft-jaju-httpbis-zstd-window-size-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Web and Internet Transport"
workgroup: "HTTP"
keyword:
 - zstd
 - zstandard
 - compression
 - content encoding
 - application/zstd
venue:
  group: "HTTP"
  type: "Working Group"
  mail: "ietf-http-wg@w3.org"
  arch: "https://lists.w3.org/Archives/Public/ietf-http-wg/"
  github: "nidhijaju/draft-zstd-window-size"
  latest: "https://nidhijaju.github.io/draft-zstd-window-size/draft-jaju-httpbis-zstd-window-size.html"

author:
 -
    fullname: Nidhi Jaju
    organization: Google
    email: nidhijaju@google.com

normative:

informative:


--- abstract

Deployments of Zstandard, or "zstd", can use different window sizes to limit
memory usage during compression and decompression. Some browsers and user agents
limit window sizes to mitigate memory usage concerns, causing interoperability
issues. This document updates the window size limit in RFC8878 from a
recommendation to a requirement in HTTP contexts.


--- middle

# Introduction

Zstandard, or "zstd", specified in {{?RFC8878}}, is a lossless data compression
mechanism similar to gzip. When used with HTTP, the "zstd" Content Encoding
token signals to the decoder that the content is Zstandard-compressed.

Deployments of Zstandard can use different window sizes to configure the maximum
memory a decoder requires to decompress a frame. Larger window sizes tend to
improve the compression ratio, but cause more memory to be used. {{RFC8878}}
provides a recommendation for decoders to support window sizes up to 8 MB, and
for encoders to not generate frames requiring window sizes larger than 8 MB.
However, it is just a recommendation ({{!RFC8878, Section 3.1.1.1.2}}).

To protect against unreasonable memory usage, some browsers and user agents
limit the maximum window size allowed. This causes incompatibilities if the
content is compressed with a larger limit, leading to decreased interoperability.

This document updates {{RFC8878}} to specify a window size limit associated with
the "zstd" Content Encoding token.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Window Size

Section 3.1.1.1.2 of {{RFC8878}} discusses window sizes in Zstandard.
The window size provides guarantees about the minimum memory buffer required to
decompress a frame. This information is important for decoders to allocate
enough memory.

The minimum window size is 1 KB. The maximum window size is (1<<41) +
7*(1<<38) bytes, which is 3.75 TB.

In general, larger window size values tend to improve the compression ratio, but
at the cost of increased memory usage.

To properly decode compressed data, a decoder will need to allocate a buffer of
at least the window size bytes.

In order to protect decoders from unreasonable memory requirements, a decoder is
allowed to reject a compressed frame that requests a memory size beyond the
decoder's authorized range.

To maintain interoperability of Zstandard in HTTP Content Encoding, decoders
MUST support window sizes of up to and including 8 MB and encoders MUST NOT
generate frames requiring a window size of larger than 8 MB, when using the
"zstd" Content Encoding token (see {{zstd-iana-token}}).

Decoders are free to support higher or lower limits, depending on local
limitations, if negotiated out-of-band. Many deployments of Zstandard operate in controlled, private environments and can directly communicate with their encoder
and decoder to negotiate a higher or lower limit.

# Security Considerations

This document introduces no new security considerations beyond those discussed
in {{RFC8878}}.

# IANA Considerations

## Content Encoding {#zstd-iana-token}

This document updates the entry added in {{RFC8878}} to the "HTTP Content
Coding Registry" within the "Hypertext Transfer Protocol (HTTP) Parameters"
registry:

Name:

: zstd

Description:

: A stream of bytes compressed using the Zstandard protocol with a window size
  of not more than 8 MB.

Reference:

: This document


--- back

# Acknowledgments
{:numbered="false"}

zstd was developed by Yann Collet.

The author would like to thank Yann Collet, Felix Handte, Klaus Post, Adam Rice,
and members of the Web Performance Working Group in the W3C for collaborating
on the window size issue and helping to formulate a solution. Felix Handte
and Nick Terrell provided feedback that went into RFC 8478 and RFC 8878.

