# OpenStack Gate Job Research
All stuffs about OpenStack Gate Job.

## Create new test jobs - Basic steps

1. Check out the [project-config](https://github.com/openstack-infra/project-config)
   repository and make it ready for patch submission like creating a branch
   where you work on.

2. Add new jobs in file `project-config/jobs/projects.yaml` - sample job:

    ```
    - '{pipeline}-<project_name>-dsvm-functional{special}-{node}{suffix}':
          pipeline: gate
          node: ubuntu-trusty  # can be ubuntu-xenial, centos-7, centos-7-2
          special: ''
          suffix: '' # -nv - non-voting
          branch-override: default
    ```

3. Now define how to trigger the job (order). Edit file
   `project-config/zuul/layout.yaml` and add new jobs:

    ```
    - name: <project_name>
      template:
        - name: template-1
        - name: template-2
      check:
        - gate-<project_name>-dsvm-deploy-centos-binary-centos-7-nv # etc.
      experimental:
        - gate-<project_name>-dsvm-deploy-centos-binary-centos-7-nv-test # etc.
    ```
4. Create a file `project-config/jenkins/<project_name>.yaml`.

5. Setup tools in <project_name> side, which was used in .yaml file (step 4).

## Multinode gate jobs - new set of testtools (draft).

1. Use Kolla-ansible as orchestration-tool to control the deployment/upgrade
   process. Multinode deployment.

2. Rolling upgrade methododology must be defined by each project. Each project
   has a different rolling upgrade strategy.

3. Create rolling upgrade test scripts.

    - Run subset of tempest smoke test?
    - Send several async requests?

## References

1. [(PS)Enable multinode gate - ocata](https://review.openstack.org/#/c/471401/)

2. [(PS)Enable multinode gate - master](https://review.openstack.org/#/c/466007/)

3. [(PS)Add new multinode gates to Kolla](https://review.openstack.org/#/c/466415/)

4. [(PS)Multinode gates, dockerhub publisher prework](https://review.openstack.org/#/c/471428/)

5. [(PS)Adding the Spec for upgrade test toolset](https://review.openstack.org/#/c/449295/) - [Github](https://github.com/ntk148v/qa-specs/blob/master/specs/other/openstack-upgrade-testing.rst)

6. [(L)An introductin to the OpenStackCI](https://www.objectif-libre.com/en/blog/2016/08/29/introduction-a-la-ci-dopenstack/)

7. [(L)OpenStack Infra Jenkins Job](http://abregman.com/2016/03/05/openstack-infra-jenkins-jobs/)

8. [(L)Understanding OpenStack CI system](http://www.joinfu.com/2014/01/understanding-the-openstack-ci-system/)

9. [(Trello)Trello link about grenade-dsvm-mutlinode-test](https://trello.com/c/lhiB7ALY/126-run-grenade-dsvm-multinode-test-successfully)

10.[(ML)Grenade multinode partial upgrade](https://openstack.nimeyo.com/65081/openstack-neutron-upgrade-grenade-multinode-partial-upgrade)

11.[Ironic Rolling upgrade](related/ironic_rolling_upgrade.md)
