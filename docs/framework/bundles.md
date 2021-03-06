# Bundles

There is no support for Symfony Bundles in Symlex as we strive to build focused, lean and fully testable applications.

  - We found that using them often adds complexity to the overall architecture: They hide bootstrap / configuration details and encourage to build bloated applications 
  - Writing meaningful unit tests is not possible, if certain functionality is exclusively encoded in framework 
    configuration files or magically generated by bundles
  - Acceptance tests could be created, but they are slow and not well suited for test driven development (TDD) and refactoring

!!! quote
    I've seen many people trying to use FOSUserBundle.
    I've been struggling with it for 6 hours now. Just to be able to make a custom user registration form. 
    The basic documentation is 6 pages long...<br>
    ― *Olivier Pons on [Stack Overflow](http://stackoverflow.com/questions/19064719/fosuserbundle-what-is-the-point)*
    
## Annotations ##

Annotations are also not supported as we find them difficult to maintain and test. We prefer a tiny bit of code instead
or use the service container to [configure](config.md) our apps.