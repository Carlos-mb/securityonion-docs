.. _cyberchef:

CyberChef
=========

:ref:`soc` includes a link on the sidebar that takes you to CyberChef. 

From https://github.com/gchq/CyberChef:

    The Cyber Swiss Army Knife
    
    CyberChef is a simple, intuitive web app for carrying out all manner of "cyber" operations within a web browser. These operations include simple encoding like XOR or Base64, more complex encryption like AES, DES and Blowfish, creating binary and hexdumps, compression and decompression of data, calculating hashes and checksums, IPv6 and X.509 parsing, changing character encodings, and much more.

    The tool is designed to enable both technical and non-technical analysts to manipulate data in complex ways without having to deal with complex tools or algorithms.
    
    There are four main areas in CyberChef:

    1. The input box in the top right, where you can paste, type or drag the text or file you want to operate on.
    2. The output box in the bottom right, where the outcome of your processing will be displayed.
    3. The operations list on the far left, where you can find all the operations that CyberChef is capable of in categorised lists, or by searching.
    4. The recipe area in the middle, where you can drag the operations that you want to use and specify arguments and options.

Screenshot
----------

.. image:: images/55_cyberchef.png
  :target: _images/55_cyberchef.png

Accessing
---------

To access CyberChef, log into :ref:`soc` and click the CyberChef hyperlink.

You can send highlighted text from :ref:`pcap` to CyberChef. When the CyberChef tab opens, you will see your highlighted text in both the Input box and the Output box.

You can send all visible packet data from :ref:`pcap` to CyberChef. When the CyberChef tab opens, it will automatically apply the ``From Hexdump`` recipe to render the hexdump that was sent.

File Extraction
---------------

Suppose you are looking at an interesting HTTP file download in :ref:`pcap` and want to extract the file using CyberChef:

- Click the :ref:`pcap` CyberChef button and CyberChef will launch in a new tab. It will then show the hexdump in the Input box, automatically apply the ``From Hexdump`` recipe, and show the HTTP transcript in the Output box.
- You may want to apply an operation from the left column. One option is to use the ``Extract Files`` operation and optionally specify certain file types for extraction. Another option is to instead remove the HTTP headers using the ``Strip HTTP headers`` operation.
- If a magic wand appears in the Output box, then CyberChef has detected some applicable operations and you can click the magic wand to automatically apply those operations. For example, CyberChef might automatically apply ``Strip HTTP headers`` and then render the file.

More Information
----------------

.. note::

    For more information about CyberChef, please see https://github.com/gchq/CyberChef.
