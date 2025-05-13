# Convertible Bond (CB) Pricing
	A brief introduction
	
### CB Business Logic
	A convertible bond is a corporate debt that can be converted to the stock of the CB issuer

CB clauses or embedded choices (options) in the order of seniority:
1. Put (or hard-put). CB holder can return the CB to the issuer for cash.
2. Provisional Put (or soft-put). Conditional put.
3. Convert. CB holder can convert the CB to stock.
4. Call (or hard-call). CB issuer can call back the CB, and then forces CB holder to convert.
5. Provisional Call (or soft-call). Conditional call.

Note:
1. No matter what happens, coupon will always be paid at accured amount.
2. When called, it is assumed the holder will always choose to convert.
3. Soft-put: Uncommon in the US, but common in China.

### CB C++ Pricer Setup
	Finite difference solving the BSM or AFV PDE

Input parameters (KEY-VALUE pairs):
- The keys and their sample values are in **convbond.json**
- The keys' literal names and sample values are in the GUI
- Can be edited directly in the parameters-file with KEY-VALUE pairs (and read into the C++ pricer)

For example, KEY for the PDE model and its VALUE in the parameters-file:
- pde_model_id	afv_convbond_pde	# see [AFV](#afv)

For soft-call valuation supported, VALUE for KEY provcall_id:
- cond_range_prob	# see [CRP](#crp)
- navin_algorithm	# see CRP
- one_touch			# see CRP
- aux_rev_binomial	# see [ARB](#arb)

Convert after called or soft-called:
- Use the CB convert-schedule to convert
- Two keys can be added in **convbond.json**
  - cvt_after_call_schd		date|ratio   # schedule
  - cvt_after_pc_schd		date|ratio   # schedule


CvtBondPx_t6m.dll exports one C-function to client:
```
// use struct to pack and fetch multiple values
// px_iv = 0, Pricing; otherwise, Implied-vol
extern "C" __declspec(dllexport) void cbPDG_orIV(int px_iv, const char* prm_fn, C_struct* xyz_o);
```

References:\
<a name="afv">AFV</a>
AFV: Ayache, Forsyth, Vetzal (2003). The valuation of convertible bonds with credit risk. Journal of Derivatives.\
<a name="crp"></a>
CRP: Liu, Guo (2020). An excellent approximation for the m out of n day provision. North American Journal of Economics and Finance.\
<a name="arb"></a>
ARB: Liu, Guo (2008). Approximating the embedded m out of n day soft-call option of a convertible bond: An auxiliary reversed binomial tree method. SSRN


