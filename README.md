Role Formula
=============

In all salt, ansible, puppet and chef, there is a problem on how to give roles to machines. In a nutshell, the problem is how to assign to machines, the packages that need to be installed with a certain config, the groups that need to be set up for those machine/packages set and finally, the users that have to be set up there.

The concept of roles is usually not used when the number of machines is of a few, but having a formula like the one here from the start would avoid to have to think a way to do this associations.


Design
------

This is the design I have thought for the role formula.

=== Basic statements ===

For the design of roles, we need to have clear that salt/top.sls is file that selects which states a machine has to, and that pillar/top.sls the pillars it has access to.

The design of the roles should be taking this carefully into account, as the states to be included in salt/top.sls should be defined through the pillars. This is actually not a problem because one state can include others, therefore, salt/top.sls will need to have:


´´´
base:
  '*':
    - role
´´´

Role state is in charge of including all the states needed for an specific role, and drive the data to them.


It is really important to make clear that all the states that are managed through roles should never be managed directly, meaning that if a role you have given a machine makes use of users state, you should not use/configure that state apart, as the pillar configuration will collide [TODO: Check how pillar merges worked exactly]


Also, in order not to loose all the flexibility pillars have with their hability to specify within different files all the config, we need to make sure that we don't loose that hability. At the same time, we should not be including neither all states nor all the config, so we will need some sort of logic to define inclusions in a natural way.

Roles should neither be all defined within one file, so we should be carefull about supporting not only other pillars but other rules. Maybe it can be handled the same way.
