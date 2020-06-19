---
description: Errors and what they mean
---

# Error Reference

<table>
  <thead>
    <tr>
      <th style="text-align:left">Error</th>
      <th style="text-align:left">Explanation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">ReferenceError: behavior is not defined <em>or</em> 
        <br />Can&#x2019;t find variable: behavior</td>
      <td style="text-align:left">
        <p>Every HASH behavior file must have a function signature of
          <br />function behavior().</p>
        <p>If it is not properly defined, you&apos;ll see this error.</p>
        <p></p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ERROR running simulation: <code>[error]</code> did not match any variant
        of untagged enum OutboundMessage</td>
      <td style="text-align:left">All messages must have a <code>to</code> and <code>type </code>- this error
        indicates the type is missing.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>D is not a function.</p>
        <p></p>
      </td>
      <td style="text-align:left">Check analysis.json - this can indicate you referenced an output that
        doesn&apos;t exist or used an incorrect operation.</td>
    </tr>
    <tr>
      <td style="text-align:left">Agent &quot;<code>[agent id]</code>&quot; doesn&apos;t have a position.</td>
      <td
      style="text-align:left">Many operations on agents require a physical location on the x,y plane
        - for example searching for neighbors. This error will be thrown if that&apos;s
        the case.</td>
    </tr>
  </tbody>
</table>



We're expanding this list with more errors, explanations, and fixes. If you encounter an error that is unclear, [let us know](https://hashpublic.slack.com/archives/C0151PYN1T4).

