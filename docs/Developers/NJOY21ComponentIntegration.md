# Integration of Modern NJOY21 Component
When a new component (i.e., module) is modernized and added to [NJOY21](https://github.com/njoy/NJOY21), we need to tell NJOY21 how to link to the component and what the new component needs to accept as arguments so that it can do what it needs to do.

In this document, I use the [THERMR](https://github.com/njoy/THERMR) component as an example. 

## Summary
In summary here are the steps that need to be done to integrate a new component into NJOY21. Each of these steps is covered in detail in this document.

1. Teach [`lipservice`](https://github.com/njoy/lipservice) how to convert input arguments to JSON, e.g., `THERMR`
2. Remove module name (`THERMR`) from `setupLegacyDirectory` call.
3. Add module name (`THERMR`) to `setupModernDirectory` call. 
4. Define modern "sequence" routine for new component.
5. Create `THERMR` class (in THERMR repository) with call operator, `operator()`, taking two `nlohmann::json` arguments.
6. Add new component (i.e., THERMR repository) as git submodule to NJOY21.


## 1. Passing input arguments to modern component
Let's look at a typical input for THERMR:

```text
thermr
  0 -22 -24 /
  0 1306 8 2 1 0 0 1 221 2 /
  350.0 450.0 /
  0.05 1.2 / 
```

In NJOY21, the input deck is read by lipservice. In order to pass the arguments to the modern component we have to convert them into a JSON object. (Arguments are passed to legacy modules in some other magical fashion.) The JSON object for this input looks like:
```json
{
  "nendf":  0,
  "nin":    -22,
  "nout":   -24,
  "matde":  0,
  "matdp":  1306,
  "nbin":   8,
  "ntemp":  2,
  "iin":    1,
  "icoh":   0,
  "iform":  0,
  "natom":  1,
  "mtref":  221,
  "iprint": 2,
  "tempr":  [ 350.0, 450.0 ],
  "tol":    0.05,
  "emax":   1.2
}
```
A few things to note:

- The keys are the same argument names as in the [NJOY2016 documentation](https://github.com/njoy/NJOY2016-manual/blob/master/njoy16.pdf) (and likely the [code](https://github.com/njoy/NJOY2016/blob/master/src/thermr.f90#L100)).
- There are no "cards" in the JSON object.
- Notice that the temperatures are turned into a JSON array `[ 350.0, 450.0 ]`.

### Conversion from `lipservice` to JSON
We are using the excellent [JSON library](https://github.com/nlohmann/json) by [Niels Lohmann](https://github.com/nlohmann) for C++ JSON capabilities. That library provides abilities to convert custom objects into a JSON object. In the `lipservice` repository is demonstration of how this is done for the THERMR module in file  (shown below).

Note that there is a `to_json` function for each THERMR card and lastly a `to_json` function for the `lipservice::THERMR` object itself.
```cpp
inline void to_json( nlohmann::json& JSON, const THERMR::Card1& card1 ) {
  JSON = { 
    { "nendf", card1.nendf.value },
    { "nin",   card1.nin.value },
    { "nout",  card1.nout.value }
  };
}

inline void to_json( nlohmann::json& JSON, const THERMR::Card2& card2 ) {
  JSON = { 
    { "matde",  card2.matde.value },
    { "matdp",  card2.matdp.value },
    { "nbin",   card2.nbin.value },
    { "ntemp",  card2.ntemp.value },
    { "iin",    card2.iin.value },
    { "icoh",   card2.icoh.value },
    { "iform",  card2.iform.value },
    { "natom",  card2.natom.value },
    { "mtref",  card2.mtref.value },
    { "iprint", card2.iprint.value }
  };
}

inline void to_json( nlohmann::json& JSON, const THERMR::Card3& card3 ) {
  JSON = { { "tempr", card3.tempr.value } };
}

inline void to_json( nlohmann::json& JSON, const THERMR::Card4& card4 ) {
  JSON = {
    { "tol", card4.tol.value },
    { "emax", card4.emax.value }
  };
}

inline void to_json( nlohmann::json& JSON, const THERMR& thermr ) {
  JSON = thermr.card1;
  JSON.update( thermr.card2 );
  JSON.update( thermr.card3 );
  JSON.update( thermr.card4 );
}
```
This all goes in [`src/lipservice/src/THERMR.hpp`](https://github.com/njoy/lipservice/blob/master/src/lipservice/src/THERMR.hpp) in `lipservice`. An include statement needs to be added to [`src/lipservice/src/to_json.hpp`](https://github.com/njoy/lipservice/blob/master/src/lipservice/src/to_json.hpp)

```cpp
#include "lipservice/src/THERMR.hpp"
```

## 2. Modifying routines to set legacy and modern directories
When NJOY21 executes, it sets up some "directories" (these are *not* the same as folders on your computer), a directory for the legacy modules and a directory for the modern components. It does this in the [`setupLegacyDirectory`](https://github.com/njoy/NJOY21/blob/master/src/njoy21/Driver/Factory/src/setupLegacyDirectory.hpp) and `setupModernDiretory` respectively.

In file [`src/njoy21/Driver/Factory/src/setupLegacyDirectory.hpp`](https://github.com/njoy/NJOY21/blob/master/src/njoy21/Driver/Factory/src/setupLegacyDirectory.hpp):
```cpp
static Directory setupLegacyDirectory( CommandLine& commandLine ){
  return ( commandLine.legacySwitch ) ?
    Directory( { "MODER", "RECONR", "BROADR", "PURR", "UNRESR", "ACER",
                  "GASPR", "HEATR", "GROUPR", "VIEWR", "MIXR", "DTFR",
                  "THERMR", "LEAPR", "RESXSR", "MATXSR", "GAMINR", "PLOTR",
                  "COVR", "WIMSR", "POWR", "CCCCR", "ERRORR" } ):
    Directory( { "MODER", "RECONR", "BROADR", "PURR", "UNRESR", "ACER",
                  "GASPR", "HEATR", "GROUPR", "VIEWR", "MIXR", "DTFR",
                  "LEAPR", "RESXSR", "MATXSR", "GAMINR", "PLOTR",
                  "COVR", "WIMSR", "POWR", "CCCCR", "ERRORR" } );
}
```
Please note that `"THERMR"` occurs in the first `Directory` function call, but not in the second. **When a modern component is added, the name of the new component needs to be removed from this second `Directory` call.**

In file `src/njoy21/Driver/Factory/src/setupModernDirectory.hpp`:
```cpp
static Directory setupModernDirectory( CommandLine& commandLine ){
  return ( commandLine.legacySwitch ) ? 
    Directory() : 
    Directory( { "THERMR" } );
}
```
**When a modern component is added, the name of the new component needs to be added to this second `Directory` call.**

## 4. Define a modern "sequence"
This part is really easy.  In file `src/njoy21/modern/Sequence/routines.hpp` just add the line `DEFINE_ROUTINE( THERMR )` at the end of the file (but before the line `#undef DEFINE_ROUTINE`).
```cpp
#define DEFINE_ROUTINE( MODULE )                                               \
  struct MODULE : public interface::Routine {                                  \
    nlohmann::json j##MODULE;                                                  \
                                                                               \
    template< typename Char >                                                  \
    MODULE( lipservice::iRecordStream< Char >& stream ){                       \
      lipservice::MODULE command( stream );                                    \
      this->j##MODULE = command;                                               \
    }                                                                          \
    void operator()( std::ostream& output,                                     \
                     std::ostream& error,                                      \
                     const nlohmann::json& args ){                             \
      njoy::MODULE::MODULE{}( std::move( this->j##MODULE ),                    \
                              output, error,                                   \
                              args );                                          \
    }                                                                          \
  };

  DEFINE_ROUTINE( THERMR )                                                      
#undef DEFINE_ROUTINE
```

## 5. Create a functor for the component
In `src/njoy21/modern/Sequence/routines.hpp` we can see (although it's a bit obscure) that when NJOY21 calls the new component, it does so by calling the call operator on an object with the same name as the new component. 

```cpp
class THERMR {
public:
  void operator()( const nlohmann::json& njoyArgs, 
                   std::ostream& output,
                   std::ostream& error,
                   const nlohmann::json& args ){
    // Do something here
  }
};
```
This call operator does everything that needs to be done for the modern component. The `njoyArgs` argument is the JSON object that was created by lipservice as described earlier. To access the different members, simply use the bracket operator:
```cpp
njoyArgs[ "iprint" ];
```
What is done with the parameters in `njoyArgs` is up to whoever wrote the component.

The `output` argument is where messages about the processing should be "printed". In Legacy NJOY, this would be the `output` file. In NJOY21, the name of this file is specified using the `-o` or `--output` command-line argument.

The `error` argument is similar to the `output` argument, except that error messages are written here. Error messages are messages written before NJOY crashes (hopefully gracefully).

The `args` argument is another JSON object that contains an arbitrary set of options that are passed to every modern component. Right now this is just a placeholder for unknown future needs. 

When that function finishes, NJOY21 continues by running the next module given in the input deck, so everything that needs to be done with the modern component must be taken care of in this call operator. Likely, this function just calls other functions to do the real work.

## 6. Adding new component as submodule to NJOY21
Once the new component has been implemented (in a separate repository) it needs to be added to NJOY21. 

Since we are currently improving our build system, the steps for this are still being developed.
