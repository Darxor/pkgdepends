# new_pkg_deps

    Code
      deps
    Output
      <pkg_dependencies>
      + refs:
        - pkg3
      (use `$resolve()` to resolve dependencies)
      (use `$solve()` to solve dependencies)

---

    Code
      deps$get_refs()
    Output
      [1] "pkg3"

---

    Code
      sort(deps$get_config()$list())
    Output
       [1] "build_vignettes"           "cache_dir"                
       [3] "cran_mirror"               "dependencies"             
       [5] "goal"                      "include_linkingto"        
       [7] "library"                   "metadata_cache_dir"       
       [9] "metadata_update_after"     "package_cache_dir"        
      [11] "platforms"                 "r_versions"               
      [13] "sysreqs"                   "sysreqs_db_update"        
      [15] "sysreqs_db_update_timeout" "sysreqs_dry_run"          
      [17] "sysreqs_lookup_system"     "sysreqs_platform"         
      [19] "sysreqs_rspm_repo_id"      "sysreqs_rspm_url"         
      [21] "sysreqs_sudo"              "sysreqs_update"           
      [23] "sysreqs_verbose"           "use_bioconductor"         
      [25] "windows_archs"            

---

    Code
      deps
    Output
      <pkg_dependencies>
      + refs:
        - pkg3
      + has resolution (+2 dependencies)
      (use `$get_resolution()` to see resolution results)
      (use `$solve()` to solve dependencies)

---

    Code
      deps
    Output
      <pkg_dependencies>
      + refs:
        - pkg3
      + has resolution (+2 dependencies)
      + has solution
      (use `$get_resolution()` to see resolution results)
      (use `$show_solution()` to see the dependencies
      (use `$get_solution()` to see the full solution results)
      (use `$draw()` to draw the dependency tree)

---

    Code
      deps$draw()
    Output
      pkg3 1.0.0 [new][bld][dl] (<size>)
      \-pkg2 1.0.0 [new][bld][dl] (<size>)
        \-pkg1 1.0.0 [new][bld][dl] (<size>)
      
      Key:  [new] new | [dl] download | [bld] build

---

    Code
      deps$get_solution()
    Output
      <pkg_solution>
      + result: OK
      + refs:
        - pkg3
      + constraints (5):
        - select pkg3 exactly once
        - select pkg1 at most once
        - select pkg2 at most once
        - pkg2 depends on pkg1: version pkg1 1.0.0
        - pkg3 depends on pkg2: version pkg2 1.0.0
      + solution:
        - pkg1
        - pkg2
        - pkg3

# async

    Code
      synchronize(deps$async_resolve()$then(function() "done"))
    Output
      [1] "done"

---

    Code
      deps$get_resolution()[, c("ref", "type", "directpkg", "package", "error")]
    Output
      # A data frame: 3 x 5
        ref   type     directpkg package error     
        <chr> <chr>    <lgl>     <chr>   <list>    
      1 pkg1  standard FALSE     pkg1    <list [0]>
      2 pkg2  standard FALSE     pkg2    <list [0]>
      3 pkg3  standard TRUE      pkg3    <list [0]>

# solve policy

    Code
      deps
    Output
      <pkg_dependencies>
      + refs:
        - pkg3
      + has resolution (+2 dependencies)
      + has solution
      (use `$get_resolution()` to see resolution results)
      (use `$show_solution()` to see the dependencies
      (use `$get_solution()` to see the full solution results)
      (use `$draw()` to draw the dependency tree)

# errors

    Code
      deps$stop_for_solution_error()
    Error <rlib_error_3_0>
      ! Could not solve package dependencies:
      * needsfuturama: Can't install dependency futurama
      * futurama: Needs R >= 3000.0

