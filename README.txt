=========================================================
  05 - DRUGS
=========================================================

WHAT IS IN HERE
---------------
The full chain plant -> active ingredient -> drug -> therapeutic
class, plus the drugs actually registered on the Egyptian market
with their retail prices.

FILES
-----
01_plant_to_drug_to_disease.csv
    114 internationally registered drugs linked to 17 species.
    Includes WHO ATC codes and therapeutic class.

02_egyptian_drugs_plant_derived.csv
    929 Egyptian pharmaceutical products, 19 active ingredients,
    priced in EGP.

SOURCES
-------
KEGG REST API - rest.kegg.jp (free for academic use)
Open-licensed community database of Egyptian medicines

USE THE CONFIDENCE COLUMN
-------------------------
The 'confidence' column in file 01 grades match quality:

    high    33 drugs   exact name match, or curated by hand and
                       verified against the species' documented
                       constituent. Trustworthy.
    medium  48 drugs   salts and derivatives. Review by eye.
    low     33 drugs   contains acetyl / dihydro / desomorphine.
                       Usually a semi-synthetic derivative, not
                       obtained from the plant directly.

Rows with match_type = curated were added by hand after searching
the full KEGG drug list for each species' documented compound:
    D08735  Visnadine        -> Ammi visnaga
    D08515  Silibinin        -> Silybum marianum
    D09135  Fenugreek seed   -> Trigonella foenum-graecum
    D06462  Castor oil       -> Ricinus communis

FIVE SPECIES HAVE NO KEGG ENTRY AT ALL
--------------------------------------
    Matricaria chamomilla   Withania somnifera
    Cymbopogon citratus     Calendula officinalis
    Salvia officinalis

Their active compounds (bisabolol, withanolides, citral, faradiol
esters, thujone) are not registered drugs in KEGG. This is a
genuine absence in the source database, not a search failure.
Guaiazulene entries were deliberately NOT linked to chamomile:
guaiazulene comes from guaiac wood, chamazulene from chamomile,
and they are different compounds.

For serious analysis, filter:
    k[k.confidence.isin(['high','medium'])]

THE DISEASE COLUMN IS MOSTLY EMPTY
----------------------------------
Only 9 drugs carry a disease link in KEGG. This is a limitation
of the KEGG database itself, not an extraction failure.

Use atc_class instead. It gives the therapeutic domain
(oncology / neurological / dermatological / gastrointestinal)
for every drug, which is cleaner and more complete than a
scattered list of individual disease names.

EGYPTIAN MARKET FIGURES
-----------------------
menthol      791 products   median EGP 67
silymarin     34 products   median EGP 47
bisabolol     21 products   median EGP 80
khellin       15 products   median EGP 30   <- native Egyptian species
atropine      15 products   median EGP 18
vincristine    5 products   median EGP 78   <- cancer therapy
diosgenin      2 products   median EGP 425

WARNING
-------
The Egyptian drug data comes from an unofficial community source.
The Egyptian Drug Authority publishes no API and its search page
is CAPTCHA protected. Verify any figure you intend to publish
against the Authority's own database by hand.
