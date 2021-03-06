FROM ensemblorg/ensembl-vep:release_95.3
MAINTAINER John Garza <johnegarza@wustl.edu>

LABEL \
    description="Vep helper image"

USER root

RUN apt-get update -y && apt-get install -y \
libfile-copy-recursive-perl \
libipc-run-perl \
wget

#necessary because some legacy cwl files still refer to vep (the current name) as variant_effect_predictor.pl
WORKDIR /
RUN ln -s /opt/vep/src/ensembl-vep/vep /usr/bin/variant_effect_predictor.pl

WORKDIR /opt/vep/src/ensembl-vep
RUN perl INSTALL.pl --NO_UPDATE

RUN mkdir -p /opt/lib/perl/VEP/Plugins
WORKDIR /opt/lib/perl/VEP/Plugins

RUN wget https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/Downstream.pm \
https://raw.githubusercontent.com/griffithlab/pVACtools/master/tools/pvacseq/VEP_plugins/Wildtype.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/CADD.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/REVEL.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/ExACpLI.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/ExACpLI_values.txt \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/LoFtool.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/LoFtool_scores.txt \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/SpliceRegion.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/dbNSFP.pm \
https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/95/dbNSFP_replacement_logic

COPY add_annotations_to_table_helper.py /usr/bin/add_annotations_to_table_helper.py
COPY docm_and_coding_indel_selection.pl /usr/bin/docm_and_coding_indel_selection.pl
COPY vcf_check.pl /usr/bin/vcf_check.pl
COPY intervals_to_bed.pl /usr/bin/intervals_to_bed.pl
COPY single_sample_docm_filter.pl /usr/bin/single_sample_docm_filter.pl

USER vep
