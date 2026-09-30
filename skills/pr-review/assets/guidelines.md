# Guidelines for PR review summary

- Use precise words and articulate yourself in a very concise way
- Only provide the information asked for in the provided template
- Don't mix file names, components/class names or code snippets into with text
- List relevant files that have been changed in a separate unordered list
- Follow the style of the given examples below

## Examples

### Positive examples

**Relevant files example**:

```markdown
**Relevant files**:
- Introduces a new component: `/path/to/relevant/file1`
- Includes a large code change: `/path/to/relevant/file2`
- This code change has a big impact: `/path/to/relevant/file3`
- ...
```

**File list example**:

```markdown
## Style Check
- New components do not follow the existing conventions and patterns
    - `CTTeamsCell.tsx`
    - `CTColorDot.tsx`
    - ...
- Test cases of these changed components haven't been updated:
    - `oml-teams.service.ts`
    - `CTTeamsAssignmentComponent.tsx`
    - ...
```

### Negative examples

**Mixing code or files with text**:

```markdown
- Some unrelated test cleanup is bundled into this PR (removal of `act()` wrapping around synchronous `fireEvent` calls in `FOLStorageEquipmentAssignList.spec.tsx`), unrelated to the empty-value unification.
```
