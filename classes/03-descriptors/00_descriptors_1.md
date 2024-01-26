# What are descriptors

```note
In materials science, descriptors are representations of the qualitative or quantitative composition, and/or structure of compounds or alloys, which play an important role in machine learning of materials data.
```

```warning
**Descriptors** are usually associated with the classic definition of machine learning *features*. However, there is a slight discrepancy between the definitions of materials scientists and data analysts. 
```

In materials science, descriptors of certain properties of materials are understood as physical or statistical indicators of materials associated with a certain property. Often, these indicators have a characteristic functional form strictly for certain materials, for example, only for cubic crystals or only for insulators.

The first more or less universal descriptors for different properties of materials is a concentrational weighted average of atomic properties over chemical composition: 

$$
\begin{aligned}
    f(composition) = \sum^n_i c_i \cdot P_i
\end{aligned}
$$

To explain we need a portion of code. 
First, we are to parse chemical formula:
```python
from pymatgen.core import Composition

f2c = lambda x: Composition(x).as_dict()

formula = 'Ni3Al'
composition_as_dict = f2c(formula)
print(composition_as_dict)
```

```note
`Output: {'Ni': 3.0, 'Al': 1.0}`
```

```python
def get_atomic_concentrations(formula: dict=None):
    n = sum(list(formula.values()))
    concs = formula.copy()
    for key in formula.keys():
        concs[key] = concs[key] / n
    return concs
```

```python
def get_avg_prop(concentrations: dict=None, prop: str=None):
    ''' concentrations = dict obj '''
    avg_prop = 0
    try:
        for symbol, concentraion in concentrations.items():
            avg_prop += get_elem_props(symbol)[prop]*concentraion
        return avg_prop
    except Exception as e:
        print(f'while {symbol} is processed {e} is occured. NaN value will be returned!')
        return np.nan
```
