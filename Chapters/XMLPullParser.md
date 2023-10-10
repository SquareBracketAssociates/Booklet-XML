## XMLPullParser
  <tagA a1='att1' a2='att2'>blah</tagA>
  <tagB>aaa<tagC>bbb</tagC>ccc</tagB>
</document>
  StartTag{document,[]}
    StartTag{tagA,[Attribute{a1,'att1'},Attribute{a2,'att2'}]}
      Text{'blah'}
    EndTag{tagA}
    StartTag{tagB, []}
      Text{'aaa'}
      StartTag{tagC, []}
        Text{'bbb'}
      EndTag{tagC}
      Text{'ccc'}
    EndTag{tagB}
  EndTag{document}
EndDocument
     XPPStartDocument
        XPPEndDocument
        TagEvent
           StartTag
           EndTag
        XPPText
	"self debug:#testTextExtractionFirstLevel"
	
	| xpp  output |
	xpp := XMLPullParser parse: '<document>
  <tagA a1=''att1'' a2=''att2''>blah</tagA>
  <tagB>aaa<tagC>bbb</tagC>ccc</tagB>
</document>'.
      output := String new writeStream.
  	[ xpp next isEndDocument ] whileFalse: [
   	    xpp isText ifTrue: [ output nextPutAll: xpp text ]].
  	self assert: output contents equals: 'blahaaabbbccc'. 	
doc := '<?xml version="1.0" encoding="iso-8859-1"?>
<FILMS>
  <FILM annee="1958">
    <TITRE>Vertigo</TITRE>
    <GENRE>Drame</GENRE>
    <PAYS>USA</PAYS>
    <MES idref="3"/>
    <ROLES>
      <ROLE>
        <PRENOM>James</PRENOM>
        <NOM>Stewart</NOM>
        <INTITULE>John Ferguson</INTITULE>
      </ROLE>
      <ROLE>
        <PRENOM>Kim</PRENOM>
        <NOM>Novak</NOM>
        <INTITULE>Madeleine Elster</INTITULE>
      </ROLE>
    </ROLES>
    <RESUME>Scottie Ferguson, ancien inspecteur de police, est sujet
au vertige depuis qu''il a vu mourir son
 collegue. Elster, son ami, le charge de surveiller sa femme, Madeleine, ayant des tendances
 suicidaires. Amoureux de la jeune femme Scottie ne remarque pas le piege qui se trame autour
 de lui et dont il va etre la victime... </RESUME>
  </FILM>
<FILMS>'.
parser := XMLPullParser parse: doc.
[ parser isEndDocument ] whileFalse: [
    parser
        if: 'FILM'
        peek: [ : found |
            Transcript show: found name , ' -> ' , found attributes asString; cr.
            parser next.
            Transcript show: parser tagName ].
    parser next ]