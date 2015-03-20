dwca-io
===========

Java library for reading and writing Darwin Core Archive files.
Formerly know as dwca-reader

## Example usage

Read an archive and display the scientific name of each records:
```java
	File myArchiveFile = new File("myArchive.zip");
	File extractToFolder = new File("/tmp/myarchive");
	Archive dwcArchive = ArchiveFactory.openArchive(myArchiveFile, extractToFolder);

	Iterator<DarwinCoreRecord> it = dwcArchive.iteratorDwc();
	DarwinCoreRecord dwc;
	// loop over core darwin core records and display scientificName
	while (it.hasNext()) {
		dwc = it.next();
		System.out.println(dwc.getScientificName());
	}
```

Read from a folder(extracted archive) and display the scientific name of each records + vernacular name(s) from the extension:
```java
	Archive dwcArchive = ArchiveFactory.openArchive(new File("/tmp/myarchive"));
	System.out
			.println("Archive of rowtype " + dwcArchive.getCore().getRowType() + " with " + dwcArchive.getExtensions().size() + " extension(s)");
	// loop over core darwin core star records
	for (StarRecord rec : dwcArchive) {
		System.out.println(rec.core().id() + " scientificName: " + rec.core().value(DwcTerm.scientificName));
		//ensure we have vernacularName extension record(s)
		if (rec.hasExtension(GbifTerm.VernacularName)) {
			for (Record extRec : rec.extension(GbifTerm.VernacularName)) {
				System.out.println(" -" + extRec.value(DwcTerm.vernacularName));
			}
		}

	}
```