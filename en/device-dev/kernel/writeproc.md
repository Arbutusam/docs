# writeproc<a name="EN-US_TOPIC_0000001053240965"></a>

-   [Command Function](#section366714216619)
-   [Syntax](#section8833164614615)
-   [Parameter Description](#section12809111019453)
-   [Usage](#section15935131220717)
-   [Example](#section79281818476)
-   [Output](#section12742311179)

## Command Function<a name="section366714216619"></a>

This command is used to write data to a specified proc file system. The proc file system supports the input of string parameters. Each file needs to implement its own method.

## Syntax<a name="section8833164614615"></a>

writeproc <_data_\>  \>\>  /proc/<_filename_\>

## Parameter Description<a name="section12809111019453"></a>

**Table  1**  Parameters

<a name="table438mcpsimp"></a>
<table><thead align="left"><tr id="row444mcpsimp"><th class="cellrowborder" valign="top" width="21.000000000000004%" id="mcps1.2.4.1.1"><p id="p446mcpsimp"><a name="p446mcpsimp"></a><a name="p446mcpsimp"></a><strong id="b16354000851164"><a name="b16354000851164"></a><a name="b16354000851164"></a>Parameter</strong></p>
</th>
<th class="cellrowborder" valign="top" width="52.970000000000006%" id="mcps1.2.4.1.2"><p id="p448mcpsimp"><a name="p448mcpsimp"></a><a name="p448mcpsimp"></a><strong id="b2720614204911"><a name="b2720614204911"></a><a name="b2720614204911"></a>Description</strong></p>
</th>
<th class="cellrowborder" valign="top" width="26.030000000000005%" id="mcps1.2.4.1.3"><p id="p450mcpsimp"><a name="p450mcpsimp"></a><a name="p450mcpsimp"></a><strong id="b3067324051164"><a name="b3067324051164"></a><a name="b3067324051164"></a>Value Range</strong></p>
</th>
</tr>
</thead>
<tbody><tr id="row451mcpsimp"><td class="cellrowborder" valign="top" width="21.000000000000004%" headers="mcps1.2.4.1.1 "><p id="p2500105121818"><a name="p2500105121818"></a><a name="p2500105121818"></a>data</p>
</td>
<td class="cellrowborder" valign="top" width="52.970000000000006%" headers="mcps1.2.4.1.2 "><p id="p1149945111817"><a name="p1149945111817"></a><a name="p1149945111817"></a>Indicates the string to be entered, which ends with a space. If you need to enter a space, use <strong id="b11296145424918"><a name="b11296145424918"></a><a name="b11296145424918"></a>""</strong> to enclose the space.</p>
</td>
<td class="cellrowborder" valign="top" width="26.030000000000005%" headers="mcps1.2.4.1.3 "><p id="p749810571812"><a name="p749810571812"></a><a name="p749810571812"></a>N/A</p>
</td>
</tr>
<tr id="row155978258237"><td class="cellrowborder" valign="top" width="21.000000000000004%" headers="mcps1.2.4.1.1 "><p id="p195983258238"><a name="p195983258238"></a><a name="p195983258238"></a>filename</p>
</td>
<td class="cellrowborder" valign="top" width="52.970000000000006%" headers="mcps1.2.4.1.2 "><p id="p25985252238"><a name="p25985252238"></a><a name="p25985252238"></a>Indicates the proc file to which <strong id="b1319518261507"><a name="b1319518261507"></a><a name="b1319518261507"></a>data</strong> is to be passed.</p>
</td>
<td class="cellrowborder" valign="top" width="26.030000000000005%" headers="mcps1.2.4.1.3 "><p id="p10598425112312"><a name="p10598425112312"></a><a name="p10598425112312"></a>N/A</p>
</td>
</tr>
</tbody>
</table>

## Usage<a name="section15935131220717"></a>

The proc file implements its own  **write**  command. After the  **writeproc**  command is called, the input parameter will be passed to the  **write**  command.

>![](public_sys-resources/icon-note.gif) **NOTE:** 
>The proc file system does not support multi-thread access.

## Example<a name="section79281818476"></a>

Enter  **writeproc test \>\> /proc/uptime**.

## Output<a name="section12742311179"></a>

OHOS \# writeproc test \>\> /proc/uptime

\[INFO\]write buf is: test

test \>\> /proc/uptime

>![](public_sys-resources/icon-note.gif) **NOTE:** 
>The uptime proc file temporarily implements the  **write**  command. The  **INFO**  log is generated by the implemented  **test**  command.
