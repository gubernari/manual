Analysis
--------

Setup
~~~~~

Ask one of the `admins <../people#admin-access>`_ to create a new repository on Github with name ``analysis-AYEAR-AIDX``.
where ``AYEAR`` is the year in which the analysis was begun and ``AIDX`` is a running index for analyses begun
in the same year..
The repository's description should list the topic and authors of the analysis in the format:

.. code-block:: none

	Description of the Physics [F. Author, S. Authors, ...]

The repository should be created as a private repository and the `admins <../people#admin-access>`_ should add the
invidivual authors as collaborators.

Purpose
~~~~~~~

The purpose of the analysis repository is to store the analysis file and ancillary files & figures generated in the course
of the analysis. Figures should be stored within the ``figures/`` directory.

Once the analysis is complete, one of the `admins <../people#admin-access>`_ releases the contents of the analysis
repository as a named dataset within the ``eos/data`` repository, see `Releasing an Analysis <#releasing-an-analysis>`_.
This yields a Zenodo DOI, which can be cited within the publication associated with the analysis.

Contents
~~~~~~~~

README
^^^^^^

The repository should contain a ``README.md`` file with the following contents:
 - title and authors of the analysis;
 - list and short description of all ancillary files;
 - table of all main and supplementary figures.

Zenodo Metadata
^^^^^^^^^^^^^^^

The repository should contain a ``.zenodo.json`` file with the following contents:
 - The title is ``EOS/DATA-DYEAR-DIDX: Supplementary material for EOS/ANALYSIS-AYEAR-AIDX``.
   Note: the dataset's ``DYEAR`` and ``IDX`` can and will likely differ from the analysis' ``AYEAR`` and ``AIDX``.
 - The list of all authors of the analysis, including affiliation and ORCID.
 - The one-sentence description of the analysis.
 - The list of all funded grants held by the analysis authors.
   Note: a grant is identified as ``"id": "DOI::ID"``,
   where ``DOI`` identifies the funding agency and ``ID`` identifies the specific grant, see
   `the Zenodo documentation <https://developers.zenodo.org/#representation>`_ for a list of supported funding agencies.

An example of a ``.zenodo.json`` file is given below:

::

   {
    "upload_type": "dataset",
    "title": "EOS/DATA-2023-01: Supplementary material for EOS/ANALYSIS-2022-05",
    "description": "Supplementary material including input files, results, and figures for EOS/ANALYSIS-2022-05, a study of the ubℓν sector of the Weak Effective Theory.",
    "license": "CC-BY-4.0",
    "creators": [
        {
            "affiliation": "Rudjer Boskovic Institute, Division of Theoretical Physics, Bijeniˇcka 54, HR-10000 Zagreb, Croatia",
            "name": "Leljak, Domagoj"
        },
        {
            "affiliation": "Rudjer Boskovic Institute, Division of Theoretical Physics, Bijeniˇcka 54, HR-10000 Zagreb, Croatia",
            "name": "Melić, Blaženka"
        },
        {
            "affiliation": "Physik Department T31, Technische Universität München, 85748 Garching, Germany",
            "name": "Novak, Filip"
        },
        {
            "orcid": "0000-0001-6033-3606",
            "affiliation": "Institute for Particle Physics Phenomenology and Department of Physics, Durham University, Durham DH1 3LE, UK",
            "name": "Reboud, Méril"
        },
        {
            "orcid": "0000-0002-7668-810X",
            "affiliation": "Institute for Particle Physics Phenomenology and Department of Physics, Durham University, Durham DH1 3LE, UK",
            "name": "van Dyk, Danny"
        }
    ],
    "grants": [
      {
        "id": "10.13039/100014013::ST/V003941/1"
      },
      {
        "id": "10.13039/501100004488::IP-2019-04-7094"
      }
    ]
   }


Releasing an Analysis
~~~~~~~~~~~~~~~~~~~~~

Once the analysis is complete, one of the `admins <../people#admin-access>`_ releases the analysis by
 - importing the analysis repository's contents to the ``eos/data`` repository as a new orphaned commit
   and tagging the commit as ``DYEAR-DIDX``, where ``DYEAR`` is the year of the release and ``DIDX`` is a running
   index for releases in the same year;

   .. code-block:: none

    $ git remote add gh-analysis-AYEAR-AIDX ssh://github/eos/analysis-AYEAR-AIDX
		$ git checkout --orphan tmp gh-analysis-AYEAR-AIDX/master
		$ git commit -m "EOS/DATA-DYEAR-DIDX: Supplementary material for EOS/ANALYSIS-AYEAR-AIDX"
		$ git tag DYEAR-DIDX
		$ git push gh refs/tags/DYEAR-DIDX

 - creating a new release within ``eos/data`` for the new tag, triggering a release on Zenodo;
 - linking the dataset in the file ``README.md`` in the ``master`` branch of ``eos/data``
