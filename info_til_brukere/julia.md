# Julia

The latest long supported released of Julia, 1.6.5, is install on the cluster. 

## Packages
Currently, the only way to install packages is from the command line shell or directly from the command line:

```bash
julia
> using Pkg
> Pkg.add("DataFrames")
```
or directly from command line 
```bash
julia -e 'using Pkg; Pkg.add("DataFrames")'
```
To install multiple packages from the command line, use the dot suffix:
```bash
julia -e 'using Pkg; Pkg.add.(["DataFrames", "Flux"])'
```

When installing packages this way, they are installed locally in your home directory in `~/Julia`.




