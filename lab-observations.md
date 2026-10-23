# Lab observations and troubleshooting notes

## Scope

This document preserves the useful operational lessons from an authorized training exercise without publishing room answers, flags, or a room-specific exploit sequence.

## Evidence labels

- **Observed:** The output was present in the conversation transcript.
- **User-reported:** The learner described the result, but raw output was not preserved.
- **Not verified:** A command or hypothesis was discussed, but no result was captured.
- **Hypothesis:** Technically plausible and worth testing, but not observed during this session.

## Observed environment and service checks

The learner connected over SSH to the THM Ubuntu machine and found the supplied PHP2Shell exercise directory under the home directory. The learner inspected the top of the supplied Python script with a pager; only its interpreter declaration was captured, so this does not establish what the full script does or whether it executes successfully.

A request to the local `/index.php` endpoint returned HTTP `200 OK`. The response included Apache and PHP technology-identification headers and a short body. Exact version values and the body text are intentionally redacted from this public edition because they were also direct questions in the room.

The target was described by the lab as a PHP service running in a container on a loopback address from the target host. The conversation did not independently inspect the container's mount configuration, so path-sharing assumptions should not be treated as verified.

## Sanitized command reference

| Command observed or discussed | Status | What it establishes / limitation |
|---|---|---|
| `ls -la` | **Observed** | Listed the Ubuntu home directory and revealed the lab directory. It did not validate the exploit. |
| `less exploit.py` | **Observed** | Opened the file in a pager. Only the shebang was captured; the script was not fully reviewed in the transcript. |
| `curl -i http://127.0.0.1:8088/index.php` | **Observed** | Returned HTTP `200 OK`, response headers, and a short body. It established that the local endpoint responded, not that it was exploitable. Challenge-specific header values and body text are omitted here. |
| `whoami` | **Observed** | Returned `ubuntu` for the SSH shell user. It does not establish the identity of a process inside the PHP container. |
| `ls -ld out` | **Observed** | Showed that the host-side output directory was owned by a web-service account and had mode `775`. |
| `ls -l out/c` | **Observed** | Reported that the file did not exist at that point. |
| Attempted heredoc write to `out/c` | **Failed** | The shell returned `Permission denied`; the ordinary SSH user lacked permission to create that entry in the directory. The lab-specific command body is intentionally not reproduced. |
| `sudo tee ... >/dev/null` followed later by `sudo cat out/c` | **Observed** | No output from `tee` was expected because standard output was redirected to `/dev/null`; `sudo cat` later displayed the saved file. The sensitive lab-specific file contents are omitted. |
| `python3 exploit.py --help` | **Suggested, not verified** | No help output was captured, so supported flags cannot be inferred from the transcript. |
| Exploit execution and flag retrieval | **Not independently verified in the transcript** | The learner reported completing the room, but no raw proof output, exit status, process identity, or flag value was preserved. |

## Troubleshooting journal

### 1. Copying terminal text

**Symptom:** Copying text from the terminal did not work.

**What we know:** The terminal type and browser/terminal shortcut were not established, so the root cause remains unknown.

**Reusable method:** Use pager navigation or bounded output (`head`, `sed`) to reduce copying complexity. Identify the terminal client before assuming a universal copy shortcut. Avoid copying large files when only a small relevant excerpt is needed.

### 2. Seeing only the script's first line in `less`

**Symptom:** `less` displayed only the interpreter declaration.

**Cause:** The pager was at the beginning of the file; it had not yet been paged down.

**Resolution:** Use Space or the arrow keys to navigate and `q` to exit. A pager reads the file; it does not run the script.

### 3. Bash displayed a `>` continuation prompt

**Symptom:** Typing `\q` or `quit()` did not return to the normal prompt.

**Cause:** Bash was collecting the rest of a multi-line heredoc because the closing delimiter had not yet been received.

**Resolution:** `Ctrl+C` interrupted the unfinished command and returned the shell to its normal prompt. Confirm the prompt before starting another command. When using a heredoc, put the exact closing delimiter alone on a line.

### 4. File creation failed with `Permission denied`

**Symptom:** Writing `out/c` as the ordinary SSH user failed.

**Evidence:** The directory was owned by a web-service account and had mode `775`; the file did not exist. In Unix permissions, `775` gives write access to owner and group, but not to other users. Group membership for the SSH user was not established in the captured output.

**Lesson:** Check `whoami`, `ls -ld DIRECTORY`, and `ls -l FILE` before modifying permissions. Do not reflexively use `chmod 777` or change ownership. Prefer the smallest scoped correction needed for the authorized lab.

### 5. `sudo tee` appeared to do nothing

**Symptom:** There was no visible output.

**Cause:** Standard output was explicitly redirected to `/dev/null`, so silence was expected.

**Verification used:** The learner subsequently read the file with `sudo cat`. This verified the saved contents on the host, but did not prove the container could read the same path.

### 6. Confusion between host paths and container paths

**Symptom:** A file created on the Ubuntu host was expected to appear at a path inside the PHP container.

**What remains unknown:** The mount configuration was not independently inspected in the transcript.

**Reusable method:** Verify the container mount and paths using authorized environment inspection or lab documentation. A file in a host directory is not automatically available inside a container unless it is bind-mounted or otherwise shared.

## General troubleshooting loop

1. Stop after an error and return to a known shell state.
2. Classify it: shell syntax, permissions, path/mount, service availability, or exploit behavior.
3. Run one minimal diagnostic that tests the current hypothesis.
4. Change only what the evidence supports.
5. Verify the change independently and capture the output.
6. Separate what the evidence proves from what it merely suggests.
