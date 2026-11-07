<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import { groupByCause, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let { parserIssue, hintIssue }: { parserIssue: string; hintIssue: string } = $props();

  const merged = uniqueById(reports as Report[]).filter((r) => r.mergedInto === 4471);
  const groups = groupByCause(merged, tenantOf);
  const issueOf = (cause: string) => cause === 'parser' ? parserIssue : cause === 'hint' ? hintIssue : null;
</script>

<p>After removing repeated export rows, <strong>{merged.length} unique reports</strong> remain in #4471. They split into <strong>{groups.length} causes</strong>:</p>

<table>
  <thead><tr><th>Cause</th><th>Reports</th><th>Tenants</th><th>First to last report</th><th>Issue</th></tr></thead>
  <tbody>
    {#each groups as group (group.cause)}
      <tr>
        <td><code>{group.cause}</code></td>
        <td>{group.reports.length}</td>
        <td>{group.tenants}</td>
        <td>{group.firstSeen} to {group.lastSeen}</td>
        <td>{#if issueOf(group.cause)}<a href={issueOf(group.cause)!}>GitHub issue</a>{:else}Needs triage{/if}</td>
      </tr>
    {/each}
  </tbody>
</table>
