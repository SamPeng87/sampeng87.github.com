---
layout: post
title: "urlencode的标准规定"
description: "urlencode"
category: 
tags: [urlencode, rfc1738]
---
{% include JB/setup %}

一直说urlencode什么的，其实并不清楚其标准来源何方，今天看资料，无意中看到RFC1738.就是专门说url规范的。其中定义了urlencode标准

    URLs are sequences of characters, i.e., letters, digits, and special
    characters. A URLs may be represented in a variety of ways: e.g., ink
    on paper, or a sequence of octets in a coded character set. The
    interpretation of a URL depends only on the identity of the
    characters used.

    In most URL schemes, the sequences of characters in different parts
    of a URL are used to represent sequences of octets used in Internet
    protocols. For example, in the ftp scheme, the host name, directory
    name and file names are such sequences of octets, represented by
    parts of the URL.  Within those parts, an octet may be represented by


    the chararacter which has that octet as its code within the US-ASCII
    [20] coded character set.

    In addition, octets may be encoded by a character triplet consisting
    of the character "%" followed by the two hexadecimal digits (from
    "0123456789ABCDEF") which forming the hexadecimal value of the octet.
    (The characters "abcdef" may also be used in hexadecimal encodings.)

    Octets must be encoded if they have no corresponding graphic
    character within the US-ASCII coded character set, if the use of the
    corresponding character is unsafe, or if the corresponding character
    is reserved for some other interpretation within the particular URL
    scheme.

**No corresponding graphic US-ASCII**:

    URLs are written only with the graphic printable characters of the
    US-ASCII coded character set. The octets 80-FF hexadecimal are not
    used in US-ASCII, and the octets 00-1F and 7F hexadecimal represent
    control characters; these must be encoded.

**Unsafe**:

    Characters can be unsafe for a number of reasons.  The space
    character is unsafe because significant spaces may disappear and
    insignificant spaces may be introduced when URLs are transcribed or
    typeset or subjected to the treatment of word-processing programs.
    The characters "<" and ">" are unsafe because they are used as the
    delimiters around URLs in free text; the quote mark (""") is used to
    delimit URLs in some systems.  The character "#" is unsafe and should
    always be encoded because it is used in World Wide Web and in other
    systems to delimit a URL from a fragment/anchor identifier that might
    follow it.  The character "%" is unsafe because it is used for
    encodings of other characters.  Other characters are unsafe because
    gateways and other transport agents are known to sometimes modify
    such characters. These characters are "{", "}", "|", "\", "^", "~",
    "[", "]", and "`".

    All unsafe characters must always be encoded within a URL. For
    example, the character "#" must be encoded within URLs even in
    systems that do not normally deal with fragment or anchor
    identifiers, so that if the URL is copied into another system that
    does use them, it will not be necessary to change the URL encoding.


**Reserved**:

    Many URL schemes reserve certain characters for a special meaning:
    their appearance in the scheme-specific part of the URL has a
    designated semantics. If the character corresponding to an octet is
    reserved in a scheme, the octet must be encoded.  The characters ";",
    "/", "?", ":", "@", "=" and "&" are the characters which may be
    reserved for special meaning within a scheme. No other characters may
    be reserved within a scheme.

    Usually a URL has the same interpretation when an octet is
    represented by a character and when it encoded. However, this is not
    true for reserved characters: encoding a character reserved for a
    particular scheme may change the semantics of a URL.

    Thus, only alphanumerics, the special characters "$-_.+!*'(),", and
    reserved characters used for their reserved purposes may be used
    unencoded within a URL.

    On the other hand, characters that are not required to be encoded
    (including alphanumerics) may be encoded within the scheme-specific
    part of a URL, as long as they are not being used for a reserved
    purpose.