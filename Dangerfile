# MR is a work in progress and shouldn't be merged yet
warn "MR is classed as Work in Progress" if gitlab.mr_title.include? "[WIP]"

#Warn when merge request is unassigned
warn "This MR does not have any assignees yet." unless gitlab.mr_json["assignee"]

# Warn when there is a big MR
warn "Big MR, consider splitting into smaller" if git.lines_of_code > 500

# Ensure a clean commits history
if git.commits.any? { |c| c.message =~ /^Merge branch '#{gitlab.branch_for_base}'/ }
  fail "Please rebase to get rid of the merge commits in this PR"
end

# Mainly to encourage writing up some reasoning about the PR, rather than
# just leaving a title
if gitlab.pr_body.length < 5
  fail "Please provide a summary in the Pull Request description"
end

# If these are all empty something has gone wrong, better to raise it in a comment
if git.modified_files.empty? && git.added_files.empty? && git.deleted_files.empty?
  fail "This MR has no changes at all, this is likely an issue during development."
end