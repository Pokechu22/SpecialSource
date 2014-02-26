SpecialSource v2
================

Introduction
------------
What you see here is the second iteration of the popular Java bytecode comparison and mapping tool - SpecialSource. Built upon the functionality of the original SpecialSource, SpecialSource v2 aims to address a number of issues experienced in SpecialSource, mainly involving code complexity issues as more and more features were added. We aim to solve this issue by planning for feature addition from the start, and using an integrated bytecode parser to prevent being restricted by limitations in other libraries.

Required Features
-----------------
The following is an exhaustive list of all the features we need before SpecialSource v2 can be considered a production ready replacement for SpecialSource and ready for use in downstream projects such as MCP, Minecraft Forge and MCPC+.

* Manipulation of the *Searge* and *Compact Searge* formats. Whenever a mapping file is specified as an argument, the type will be determined based on the extension being either `.srg` or `.csrg` for *Searge* and *Compact Searge* respectively.
`searge in.srg out.csrg --reverse`
If the input and output formats are different, they will be converted upon dumping. When the `--reverse` option is specified, the mappings will become reversed. Due to limitations of the format, this will fail if the input is a *Compact Searge* formatted file.

* Comparison of two identical jars with different symbol naming.
`compare first.jar second.jar out.srg --complete --compact`.
By default this will output the results in the *Searge* mapping format, and omit identical symbols. The `--complete` option will include these identical symbols. This command will fail if any inconsistancies are detected in the structure of the provided classes, ie: class count, field count, method count.

* Complex remapping of jar files based on inputted mappings and transformers. This method must support *Searge, Compact Searge, MCP CSV, Maven Shade and Access Transformer* style mappings. This will be done by way of chaining, where the parsed output from each format will be the input for the next format. A logical use of this would be transforming obfuscated names to numeric names, then altering their access before finally outputting in a human readable format based on the *MCP CSV* data. It is also important that several exotic functions from SpecialSource be supported. These include using the classpath for inheritance lookup: `--live`, removing attributes from the generated class file: `--kill`, adding magic values to the constant pool: `--identifier`, and excluding specific packages from being remapped: `--exclude`. A complete remap invocation would look similar to the following:
`remap input.jar output.jar client.srg fields.csv methods.csv com/google/guava=net/md_5/guava access.at --live --kill LocalVariableTable --kill SourceFile --identifier SpecialSource-Remapped --exclude net/md_5`
The format of the mapping chain inputs is deduced based on the file extension or format specified.