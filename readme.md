# Transitive dependency exclusions (Maven vs. Gradle)

Given the following dependency graph, what do you think, which version of guava should be in the classpath?

```

(our project) ------->  (A)   ------->  (guava 11)
      |   \
      |    ---------->  (B)   ---- exclude guava 27 --->  (C)   ------->  (guava 27)
      \
       -------------->  (D)   ------->  (C)   ------->  (guava 27)    

```

It turns out, that if we use Maven, then we got guava 11. Maven seems to apply the exclusion of guava 27, even when it gets imported transitively through a path where the exclusion was not defined.

However, when we use Gradle, then we end up with guava 27 on the classpath. 



## try out for yourself

Prerequisites:
- install maven and gradle
- locally install the 4 dependencies to the maven repo by executing `mvn install` in the dependency project folders (use sequence A, C, B, D)

Use the following commands to regenerate the dependency trees:
```bash
cd maven-test
mvn dependency:tree > ../maven_dependencies.txt
mvn dependency:resolve >> ../maven_dependencies.txt

cd ../gradle-test
gradle dependencies > ../gradle_dependencies.txt
```