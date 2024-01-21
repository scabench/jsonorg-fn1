## json.org CVE-2022-45688 false negative

The project illustrates [CVE-2022-45688](https://nvd.nist.gov/vuln/detail/CVE-2022-45688)  in [json.org](https://mvnrepository.com/artifact/org.json/json/20220924) -- there is a simple application
`XML2JSONConverter` to read XML from input, convert it to JSON and 
pretty-print it to the console. 

Using malicious input, the application crashes with a stackoverflow. 

The test case `CVE202245688Test` illustrates this behaviour, it 
can be executed by running `mvn test`.

### Running Software Composition Analyses

There are several sh scripts to run different analyses, result resports can be found in `scan-results`.

### False Negative

The SCA report (including the onces set up as GitHub actions) fail  to report [CVE-2022-45688](https://nvd.nist.gov/vuln/detail/CVE-2022-45688), although
the included test clearyly shows that it exists. The reason is that tools based on 
meta-data analysis only (i.e. declared dependencies) dont realise that the project
uses a [shaded version of (a vulnerable version of) json.org](https://mvnrepository.com/artifact/org.json/json/20220924).

Note that shading is done manually, i.e. the Maven shade plugin is not used.
[Our recent research](https://arxiv.org/abs/2306.05534) suggests that this is a common source of sofwtare composition analysis
false negatives.

### Generating the SBOM

The `pom.xml` has a plugin to generate a [SBOM](https://www.cisa.gov/sbom) in [CycloneDX](https://cyclonedx.org/) format. 
To do this, run `mvn cyclonedx:makePackageBom`, the SBOM can be found in 
`target/` in `json` and `xml` format.

