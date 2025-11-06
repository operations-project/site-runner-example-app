# site-runner-example-app
An example repo containing server configuration and app code.

## Instructions

This repo was setup in concert with writing the instructions from https://github.com/operations-project/ansible-collection-site-runner/tree/feature/how-to?#how-to

## Variables

To get started you need the following bits of info.

- Server Hostname. Use a FQDN with a DNS record for ease of use.
- GitHub usernames of your server admins.
- Name of your app repository.
- A GitHub token with repo admin privileges. Used for creating self-hosted runners.

To set this up for your own host, just copy this repo and change the following files in the [`ansible`](./ansible) directory:

- [`hosts`](./ansible/hosts) - Define server hostname. Defines your Ansible inventory. See [Ansible Documentation](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html).
  ```ini
  [operations_host_ddev]
  server.mydomain.com ansible_connection=local
  ```
- [`group_vars/all.yml`](./ansible/group_vars/all.yml) - Define server admins. Add github usernames.
  ```yml
  operations_admin_users:
  - jonpugh
  ```
- [`host_vars/server.mydomain.com.yml`](./ansible/host_vars/server.mydomain.com.yml) - Define which repos go on the server. Rename to match your hostname.
  ```yml
  operations_github_runners:
  - runner_repo: operations-project/site-runner-example-app
  ```

Set the secret variable `operations_github_api_token` to the GitHub token using your preferred secrets management tools.

For example, to use github secrets, you can pass it on the command line in your workflow file:
```
ansible-playbook --extra-vars operations_github_api_token=${{ secrets.REPO_ADMIN_TOKEN }}
```

See [`site-runner.test.yml`](./.github/workflows/site-runner.test.yml#53)
