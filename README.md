# Ansible Gitlab Runner Windows

# Notes
An understanding of GitLab CI Runners is expected. Feel free to 
post any GitLab/GitLab CI on Windows questions in the Issues, I will gladly respond.

Use along with `https://github.com/rneu31/ansible-vagrant-windows-guest` for
testing.

# Explanation of Vars
- `gitlab_ci_url`: the URL to your GitLab CI server. When running
  the `gitlab-runner.exe register` command manually, it shows as an 
  example a url without `/ci`.
- `registration_token`: The GitLab token for the project the runner is to
   be registered to. This can be found on the CI/CD settings in the GitLab
   project.
- `token`: This is the token identity for the GitLab runner itself. For modern
   systems where GitLab runners are destroyed and re-spun regularly, it can
   create a mess in GitLab. For example, if a runner was registered and then
   the box was destroyed, GitLab would still have an entry for that runner.
   GitLab will correctly detect that the old/destroyed runner is no longer
   active, which will prevent GitLab from dispatching jobs to a runner that
   is no longer alive, but GitLab will not remove the token itself, creating
   a "mess".

If running this playbook for the first time (without having existing
GitLab Runner tokens you're interested in re-using) it is advised to:
- Comment out the `--token {{ token }} ` line in "configure GitLab Runner"
   to register the runner for the first time. 
- Record the full GitLab CI Runner token by navigating to the project that
  it is registered to, clicking the short version of the token, and then
  record the long version of the token.
- Place that token in `playbook.yml` `token` variable and restore the 
  `--token {{ token }}` line.
- Destroy the machine that is currently registered as the runner (`vagrant destroy` if using the Vagrant box previously mentioned).
- Re-create the machine (`vagrant up`) and run the playbook to verify 
  the GitLab runner is registered as the specified token.

